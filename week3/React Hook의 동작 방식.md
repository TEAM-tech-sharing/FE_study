> 추후 수정, 개선되어야 할 글입니다! 이 글은 작성자가 [[번역] 심층 분석: React Hook은 실제로 어떻게 동작할까?](https://hewonjeong.github.io/deep-dive-how-do-react-hooks-really-work-ko/)을 읽으며 이해한 부분을 정리한 글입니다.

Hooks는 React에서 처음 도입되어, Vue나 Svelte와 같은 다른 프레임워크에 광범위하게 도입되었으며 심지어는 일반적인 함수형 JS에서도 응용되고 있습니다. 하지만 Hook의 함수 기반 설계를 이해하려면 먼저 Javascript **클로저**에 대해 이해하고 있어야 합니다.

## 작성 이유가?

사실 React를 계속 써오면서도, 정확히 hooks가 어떻게 동작하는지 이해한다고는 말하기 어려웠습니다. 프레임워크 작동 방식보다 Javascript 기본 문법이 더 중요하다고 생각해서, 소홀히 하고 있었던 부분인데 이번 기회에 작동 방식에 대해 확실하게 이해하고자 함입니다.

## 클로저란?

![](https://images.velog.io/images/ken1204/post/ca8f839e-fbda-46d3-9ef3-0cac9c46ee1e/image.png)

- 출처: [[번역] 심층 분석: React Hook은 실제로 어떻게 동작할까?](https://hewonjeong.github.io/deep-dive-how-do-react-hooks-really-work-ko/)

> 클로저: 함수가 속한 렉시컬 스코프를 기억하여 함수가 렉시컬 스코프 밖에서 실행될 때에도 이 스코프에 접근할 수 있게 하는 기능

Hooks가 내세우는 많은 장점 중 하나는 **클래스와 고차 컴포넌트의 복잡성을 피할 수 있다**는 것입니다. 그러나 hooks를 사용하면 **bind**된 컨텍스트에 대해 걱정할 필요가 없는 대신, **클로저**에 대해 고려를 해야 합니다. 다음 예를 확인해보죠.

```js
function useState(initialValue) {
  var _val = initialValue; // _val은 useState에 의해 만들어진 지역 변수입니다.
  function state() {
    // state는 내부 함수이자 클로저입니다.
    return _val; // state()는 부모 함수에 정의된 _val을 참조합니다.
  }
  function setState(newVal) {
    // 마찬가지
    _val = newVal; // _val를 노출하지 않고 _val를 변경합니다.
  }
  return [state, setState]; // 외부에서 사용하기 위해 함수들을 노출
}
var [foo, setFoo] = useState(0); // 배열 구조분해 사용
console.log(foo()); // 0 출력 - 위에서 넘긴 initialValue
setFoo(1); // useState의 스코프 내부에 있는 _val를 변경합니다.
console.log(foo()); // 1 출력 - 동일한 호출하지만 새로운 initialValue
```

이 코드는 `useState` Hook의 아주 기본적인 형태의 복사본입니다. 이 함수에는 `state`과 `setState`라는 두 개의 내부 함수가 있습니다. `state`는 상단에 정의된 지역 변수 `_val`을 반환하고, `setState`는 전달된 매개 변수(`newVal`)로 지역 변수를 설정합니다.

### 클로저가 hooks의 어디서 사용되는 건가요?

여기서 클로저가 사용되는 부분은, 내부 함수 `state()`이 외부 함수 `useState()`에 선언된 변수 `_val`을 참조하는 경우입니다. `useState()` 함수가 먼저 호출되어 외부 함수 `useState()`은 실행 컨텍스트에서 '팝'이 됩니다. 함수의 생명주기가 끝나게 되는 것입니다. 이후 내부 함수가 호출되고, 값을 `return` 합니다. 그런데 생명 주기가 끝난 함수(`useState()`) 내에서 선언된 변수 `_val`를 내부 함수 `state()`이 참조하게 됩니다. 내부 함수 `state()`이 `return` 해야 되는 값은 `_val`입니다.

> 생명주기가 끝난 함수의 내부에서 선언된 변수를 참조하는게 가능할까요?

그런데 맨 아래쪽 코드와 같이 `console.log(foo())`를 실행할 경우 정상적으로 `0`이 출력됩니다. 생명주기가 끝난 외부 함수에서 선언된 변수 `_val`을 내부 함수 `state()`이 정상적으로 참조하고 있습니다. 이러한 참조를 바로 **<span style="background-color: lightyellow;">클로저</span>**라고 합니다.

## 함수형 컴포넌트에서 사용하기

새로 만든 `useState` 복제본을 이용해 함수형 컴포넌트 `Counter`를 만들어 보겠습니다!

```js
function Counter() {
  const [count, setCount] = useState(0); // 위에 useState와 같음
  return {
    click: () => setCount(count() + 1),
    // render 메서드는 실제 렌더링 대신 console.log로 대체했습니다.
    render: () => console.log('render:', { count: count() }),
  };
}
const C = Counter();
C.render(); // render: { count: 0 }
C.click();
C.render(); // render: { count: 1 }
```

이 코드에서 `count` 상태에 접근하기 위해 getter 함수를 호출하는 것은 실제 React의 `useState` hook의 API와 다릅니다. 이걸 한번 더 수정해보겠습니다.

## 오래된 클로저 (Stale Closure) 문제

실제 React API와 동일하게 만들려면 state가 함수가 아닌 **변수**여야 합니다. 단순히 **<span style="background-color: lightyellow;">\_val을 함수로 감싸지 않고 노출하면 버그가 발생합니다.</span>**

```js
function useState(initialValue) {
  var _val = initialValue;
  // state() 함수 없음
  function setState(newVal) {
    _val = newVal;
  }
  return [_val, setState]; // _val를 그대로 노출
}
var [foo, setFoo] = useState(0);
console.log(foo); // 함수 호출 할 필요 없이 0 출력
setFoo(1); // useState의 스코프 내부에 있는 _val를 변경합니다.
console.log(foo); // 0 출력 - 헐!!
```

이것은 오래된 클로저 문제의 한 형태입니다. `useState`의 결과에서 상태 `foo`를 비구조화할 때 초기 `useState` 호출에서 `_val`을 참조하고.. 다시는 변경되지 않습니다! `foo`는 한번 가져오고 끝난 값입니다. (정확히 왜지? 왜 이런 식으로 동작하는지 아직 이해 못함)

이상합니다. 일반적으로 현재 상태를 반영하기 위해 컴포넌트의 상태가 필요하지만, **이는 함수 호출이 아닌 단지 변수일 뿐입니다!** 이 두 가지 목표는 절대 같이 갈 수 없는 것처럼 보입니다.

## 모듈 내부의 클로저 (이해 미흡)

그렇다면 이 상황을 어떻게 해결할 수 있을까요? 이는 클로저를 또 다른 클로저 내부로 이동시켜서 해결할 수 있습니다!

```js
const MyReact = (function () {
  let _val; // 모듈 스코프 안에 state를 잡아놓습니다.
  return {
    render(Component) {
      const Comp = Component();
      Comp.render();
      return Comp;
    },
    useState(initialValue) {
      _val = _val || initialValue; // 매 실행마다 새로 할당됩니다.
      function setState(newVal) {
        _val = newVal;
      }
      return [_val, setState];
    },
  };
})();
```

즉시 실행 함수 `MyReact` 내부에서 `_val`을 선언합니다. 이제 `useState()` 내부에서 `_val`은 매 실행마다 새로 할당됩니다. `useState()`도 이제 내부 함수가 되었고, 외부 함수 `MyReact` 내부에서 선언된 `_val`을 참조하게 됩니다. (클로저가 중첩됩니다)

> 즉시 실행 함수: 함수 표현(Function expression)은 함수를 정의하고, 변수에 함수를 저장하고 실행하는 과정을 거칩니다. **<span style="background-color: lightyellow;">하지만 즉시 실행 함수는 함수를 정의하고 바로 실행</span>**하여 이러한 과정을 거치지 않는 특징이 있습니다. 함수를 정의하자마자 바로 호출하는 것을 즉시 실행 함수라고 이해하면 편할 것 같습니다. - 출처: [[자바스크립트] 함수(Function), 즉시실행함수(Immediately-invoked function expression)](https://beomy.tistory.com/9)

```js
function Counter() {
  const [count, setCount] = MyReact.useState(0);
  return {
    click: () => setCount(count + 1),
    render: () => console.log('render:', { count }),
  };
}
let App;
App = MyReact.render(Counter); // render: { count: 0 }
App.click();
App = MyReact.render(Counter); // render: { count: 1 }
```

이제 예상했던 대로 코드가 정상적으로 작동하는 것을 확인할 수 있습니다!

## `useEffect` 복제하기 (이해 미흡)

`useState` 다음으로 많이 사용되는 React hook은 `useEffect`입니다. 다음 코드를 확인해봅시다.

```js
const MyReact = (function () {
  let _val, _deps; // 스코프 안에서 상태와 의존성을 잡아 놓습니다.
  return {
    render(Component) {
      const Comp = Component();
      Comp.render();
      return Comp;
    },
    // 여기!
    useEffect(callback, depArray) {
      const hasNoDeps = !depArray;
      const hasChangedDeps = _deps
        ? !depArray.every((el, i) => el === _deps[i])
        : true;
      // 의존성 배열이 []거나, [count] 같이 상태값이 들어있다면 = 의존성이 있다면
      if (hasNoDeps || hasChangedDeps) {
        // 콜백 함수를 실행 시키고
        callback();
        // _deps 변수는 depArray가 됩니다.
        _deps = depArray;
      }
    },
    useState(initialValue) {
      _val = _val || initialValue;
      function setState(newVal) {
        _val = newVal;
      }
      return [_val, setState];
    },
  };
})();

// 실습
function Counter() {
  const [count, setCount] = MyReact.useState(0);
  MyReact.useEffect(() => {
    console.log('effect', count);
  }, [count]);
  return {
    click: () => setCount(count + 1),
    noop: () => setCount(count),
    render: () => console.log('render', { count }),
  };
}
let App;
App = MyReact.render(Counter);
// 이펙트 0
// render {count: 0}
App.click();
App = MyReact.render(Counter);
// 이펙트 1
// render {count: 1}
App.noop();
App = MyReact.render(Counter);
// // 이펙트가 실행되지 않음
// render {count: 1}
App.click();
App = MyReact.render(Counter);
// 이펙트 2
// render {count: 2}
```

의존성을 추적하기 위해(의존성이 변경될 때 `useEffect`가 재실행되므로), 이를 추적하는 별도의 변수, `_deps`를 추가하였습니다.

## React hooks는 결국 배열이다 (이해 미흡)

Hooks는 결국 배열입니다. 다음 코드를 확인해봅시다.

```js
const MyReact = (function () {
  let hooks = [],
    currentHook = 0; // Hook 배열과 반복자(iterator)!
  return {
    render(Component) {
      const Comp = Component(); // 이펙트들이 실행된다.
      Comp.render();
      currentHook = 0; // 다음 렌더를 위해 초기화
      return Comp;
    },
    useEffect(callback, depArray) {
      const hasNoDeps = !depArray;
      const deps = hooks[currentHook]; // type: array | undefined
      const hasChangedDeps = deps
        ? !depArray.every((el, i) => el === deps[i])
        : true;
      if (hasNoDeps || hasChangedDeps) {
        callback();
        hooks[currentHook] = depArray;
      }
      currentHook++; // 이 Hook에 대한 작업 완료
    },
    useState(initialValue) {
      hooks[currentHook] = hooks[currentHook] || initialValue; // type: any
      const setStateHookIndex = currentHook; // setState의 클로저를 위해!
      const setState = newState => (hooks[setStateHookIndex] = newState);
      return [hooks[currentHook++], setState];
    },
  };
})();
```

기본적인 개념은 Hook의 배열과 **각 Hook이 호출될 때 증가하고 컴포넌트가 렌더링 될 때 초기화하는 인덱스를 가지는 것입니다.**

## Hook의 규칙에 대하여 (이해 미흡)

Hook의 규칙 중 첫 번째인 ["최상위에서만 Hook을 호출해야 합니다."]에 대해 공식 문서에서 본 적이 있을 것이다. 위의 코드에서도 `currentHook` 변수(인덱스)를 사용하여 컴포넌트가 렌더링 될 때마다 **항상 동일한 순서**로 Hook이 호출되는 것이 보장됩니다.

## 결론

아직 이해가 완벽하게 되지 않은 코드가 여럿 있다. 특히, **오래된 클로저 문제** - 이 부분이 이해가 확실하게 되지 않아서 다른 부분도 이해가 부족한 것 같다.

### 참조 링크

- [[번역] 심층 분석: React Hook은 실제로 어떻게 동작할까?](https://hewonjeong.github.io/deep-dive-how-do-react-hooks-really-work-ko/)
- ['Getting Closure on React Hooks' 정리](https://rinae.dev/posts/getting-closure-on-react-hooks-summary)
- [Hook의 규칙](https://ko.reactjs.org/docs/hooks-rules.html#gatsby-focus-wrapper)
