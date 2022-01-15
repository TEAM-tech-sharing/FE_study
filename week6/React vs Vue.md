## 왜 이런 글을 작성하나요?
사실, 정말 부끄럽게도 나는 그동안 누군가가 '왜 프로젝트에 React를 적용한 거에요?' 라고 물어보면 딱히 할 말이 없었다. 왜냐고? 배운게 React 뿐이었으니까.. React를 숙달하기도 바쁜 현실이었다. 심지어 그렇다고 지금 React의 장점을 십분 활용해서 사용하고 있는 것도 아니다.

> 사실, 모든 Framework, Library를 선택하는데는 이유가 필요하다고 생각한다. 

이러한 질문은 아마(지극히 개인적인 생각입니다!😊) 문제 해결을 위해서 프레임워크 또는 라이브러리를 사용하게 되는데, 만약 이유를 모르고 사용하게 된다면 **문제 해결에 있어 최선의 결과를 도출하지 못한다고 생각한다.** (도입 비용, 도입 시 장/단점 등)

대표적으로, 자신이 SPA(Single Page Application)를 만들고 있다고 해보자. SPA를 만들기 위해서는 **상태 관리**를 통해서 화면을 렌더링 해야 하고, 이러한 부분을 지원하는 대표적인 라이브러리는 React가 있다. 그러나, SPA를 만드는 데는 Vue.js 또한 효과적이다. 오히려 입문자들에게는 Vue.js가 더 효과적일 수 있다.

이렇게 **<span style="background-color: lightyellow; font-weight: bold">"왜? 나는 React를 사용하고 있는가"</span>** 라는 궁금증이 꼬리를 물다가, 현재 국내에서 JS 라이브러리, 프레임워크 중 가장 사용자가 많은 React와 Vue의 공통점과 차이점은 뭔지 궁금해지게 됐다. 

(무지성으로 React를 사용했던 저를 반성합니다..😥)

### 근데 왜 굳이 React랑 Vue만 비교하는거야?

물론, 프론트엔드 라이브러리(혹은 프레임워크)는 React, Vue, Angular, Svelte 등이 있다. 그 중 Svelte는 아직 사용이 미미한 것 같아서 제외하였다. 

Angular를 뺀 이유는 지극히 나의 역량 부족이다. Vue.js도 처음 접하는 거라 차이점을 이해하는데 시간이 오래 걸릴 것 같은데, Angular의 차이점까지 공부하기에는 시간이 너무 오래 걸릴 것 같았다. 추후에 Angular까지 포함해서 포스팅 하도록 하겠다.

![](https://images.velog.io/images/ken1204/post/3cec4716-ba16-46b6-83a4-4d696724d496/image.png)

## React와 Vue의 공통점
먼저, React와 Vue의 공통점은 무엇인지 살펴보자.

Vue.js의 공식 문서의 '다른 프레임워크와의 비교'를 보면 다음과 같이 나온다.

- **가상 DOM**을 활용합니다.
- 반응적이고 조합 가능한 **컴포넌트**를 제공합니다.
- **코어 라이브러리에만 집중**하고 있고 **라우팅 및 전역 상태를 관리하는 컴패니언 라이브러리**가 있습니다.

뭔 소린가 싶다. 다음을 보자.

### 1. 가상 DOM
먼저, 가상 DOM을 이해해야 한다. 가상 DOM은 실제 DOM의 가벼운 사본이다. 우선, 브라우저가 DOM을 이용해 화면을 렌더링하는 방법은 다음과 같다.

1. 브라우저는 HTML 태그를 파싱하여 DOM트리를 만든다. 

2. DOM 트리와 스타일 규칙(CSS)가 합쳐져 렌더 트리를 만든다.

기존에는 화면의 변경사항을 DOM을 직접 조작해 브라우저에 반영하였다. 이럴 경우, DOM 트리가 수정될 때마다 실시간으로 렌더 트리가 생성되고, **불필요한 렌더링 작업이 반복적으로 일어나게 된다.** 

그래서 화면에 변화가 있을 때마다 실시간으로 DOM 트리를 수정하지 않고, 변경사항이 모두 반영된 가상 DOM을 만들어 한 번만 DOM 수정을 한다. 결과적으로 브라우저는 **<span style="background-color: lightyellow">"한번만 렌더링"</span>**을 할 수 있게 된다.

> React와 Vue.js는 둘다 이러한 가상 DOM을 사용해서 렌더링을 한다.

1. 상태값이 업데이트 되면, 전체 UI를 가상 돔에 리렌더링 한다.

2. 이전의 가상돔 내용과 현재 가상돔 내용끼리 비교한다.

3. **변경된 부분만 실제 돔에 렌더링한다.**

### 2. 컴포넌트

위의 가상 DOM을 통한 렌더링에서 이어지는 방향이다. 컴포넌트 단위로 변경 사항을 감지하고 **렌더링이 필요한 부분만** 바꾸어준다.

몇 가지 제한사항도 닮은 점이 있다. 각 컴포넌트의 HTML에 해당하는 코드는 단일 노드로 감싸져 있어야 한다던지, 배열을 출력 시 각각의 항목은 구분할 수 있는 고유한 `key`가 있어야 한다던지 등이다.

### 3. 코어 라이브러리에만 집중, 라우팅 및 전역 상태를 관리하는 컴패니언 라이브러리

React를 사용하든, Vue를 사용하든 라우팅 기능을 추가해야할 때 모두 별도의 라우팅을 지원하는 라이브러리를 사용할 것이다. 

- React: react-router-dom
- Vue: vue-router 

같은 경우로 말이다.

그럼 전역 상태 관리는 어떠한가? React와 Vue 모두 프레임워크(혹은 라이브러리) 내부 기능으로 포함되어 있는 것이 아니라, 따로 라이브러리를 사용한다. 아래는 각각의 경우에서 가장 대중적인 라이브러리이다. (React의 경우 전역 상태관리를 위해 Context API를 자체적으로 지원하지만, 보다 대중적인건 Redux이다)

- React: Redux
- Vue: Vuex

> 지금까지는 React와 Vue의 공통점에 대해 확인하였다. 그렇다면 차이점은 어떤 것들이 있을까?

## React와 Vue의 차이점

필자는 React가 보다 익숙하므로 React의 관점에서 Vue를 비교하는 방식으로 글을 작성할 예정이다.

### 1. JSX vs Template

Vue 2.0은 React의 클래스형 컴포넌트의 문법과 유사하기에 해당 문법으로 비교하도록 하겠다.

```js
<div id="app"></div>

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      message: 'Hello React.js!'
    };
  }
  reverseMessage() {
    this.setState({
      message: this.state.message.split('').reverse().join('')
    });
  }
  render() {
    return (
      <div>
        <p>{this.state.message}</p>
        <button onClick={() => this.reverseMessage()}>
          Reverse Message
        </button>
      </div>
    )
  }
}
ReactDOM.render(App, document.getElementById('app'));

```

버튼을 클릭하면 메시지의 내용을 거꾸로 바꾸는 내용의 React 코드다. React에서는 개발자가 **JSX**를 사용하여 JS에서 DOM을 생성한다. 

```html
<div id="app">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('');
    }
  }
});

```
Vue에서는 **HTML 파일**에 마크업을 작성하는게 기본 설정이다. (템플릿 방식) 

템플릿 방식은 전통적인 웹 개발 패러다임과 유사하므로 입문 개발자가 더 이해하기 쉽다. 숙련된 개발자 입장에서도 로직과 레이아웃을 분리할 수 있어 선호되는 방식이기도 하다.

그러나 템플릿 방식은 표준 HTML 이외에 **추가적인 HTML 구문을 학습해야 한다는 단점**이 있다. 물론, JSX 또한 플레인 Javascript에 문법이 추가된 경우라 따로 공부해야 하는 것은 마찬가지라, 러닝 커브에 관련한 부분은 비슷비슷하다.

추가적으로, Vue에서는 버전 2.x 부터 Template 방식과 render() 방식을 모두 지원하고 있다. 하지만 기본적으로는 템플릿 이용을 더 간단한 대안으로 제공하고 있다.

### 2. 컴포넌트 범위의 CSS

컴포넌트를 여러 파일(ex: CSS Module)을 통해 배포하지 않는 한 React의 CSS 범위 지정은 **CSS-in-JS** 방식을 사용한다. (ex: styled-components, emotion 등) 

이는 스타일 파일 관리가 쉬워지며, 컴포넌트 지향 스타일링에 매우 적합한 방식이지만, 스타일이 제대로 작동하기 위해서는 번들에 작성한 스타일이 모두 포함되게 된다. 스타일을 작성하는 동안 JS의 여러 기능을 능동적으로 사용할 수 있지만, 증가한 번들 크기 및 런타임 비용이 단점으로 꼽힌다.

물론 Vue도 styled-components-vue, vue-emotion 등으로 이러한 스타일링을 지원하지만, 가장 큰 차이점은 다음과 같다.

> Vue에서는 **기본 스타일링 방식**으로 싱글 파일 컴포넌트 내에서 익숙한 `style` 태그를 사용한다는 것이다.

```html
<style scoped>
  @media (min-width: 250px) {
    .list-container:hover {
      background: orange;
    }
  }
</style>
```

옵션인 `scoped` 속성은 자동적으로 엘리먼트에 **유일한 속성**(예 : `data-v-21e5b78`)을 추가해서 `.list-container:hover`를 `.list-container[data-v-21e5b78]:hover`로 컴파일한다.

유일한 속성? React를 사용하는 사람들이라면 styled-components를 사용할 때, 라이브러리가 자동적으로 **유니크한 클래스명**을 지원하는 것을 알 수 있을 것이다. Vue는 이러한 기능을 자체적으로 제공한다.

### 3. 단순성

#### 트랜스파일링

간단한 Vue 프로젝트는 별도의 변환작업 (트랜스파일링) 없이도 브라우저에서 바로 동작이 된다. - Babel의 도움 없이도 가능하다는 뜻이다.

React도 이런게 가능은 하지만, 일반적인 React 코드는 JSX와 ES6 문법에 의존을 많이 하고 있기 때문에, 필수적으로 별도의 변환 작업이 필요하다.

#### state

추가적으로, React와 Vue의 상태관리 방식을 비교해보자. 

React에서 state은 **불변(immutable)**의 속성을 가진다. 그래서 직접적으로 변경을 하면 안되고, 상태를 변경하고 싶다면 아래처럼 `setState()`를 사용해야 한다.

```js
this.setState({
    message: this.state.message.split('').reverse().join('')
});

```

React는 현재와 이전 상태를 비교하여 언제, 어떻게 다시 DOM을 렌더링 할지 결정한다. 그렇기 때문에 불변하는 속성이 필요하다.

반대로, Vue 에서 **data 는 변경될 수 있다.** 아래는 위의 React 의 `message`와 같은 data 속성을 직접 변경하는 모습이다.

```js
// 아래의 message 속성은 Vue 인스턴스에서 접근 할 수 있습니다.
this.message = this.message.split('').reverse().join('');

```

Vue는 실제로 렌더링을 할 때, state에 새로운 객체가 추가되었을 때, Vue가 해당 객체의 모든 속성을 확인한 뒤 getter와 setter로 변환한다. 

> **여기서 Vue의 reactivity system이 모든 상태를 모니터링하여 변경이 일어날 때마다 자동으로 DOM을 다시 렌더링한다.**

Vue의 렌더링 시스템은 React보다 기본적으로 더 빠르고 효율적이다. (상태의 불변성을 지키지 않음에도 불구하고! - 정확히 말하면 지킬 필요가 없지만) 

그러나 Vue는 속성 추가, 삭제 등 특정 배열에 대한 변경을 감지하지 못한다. 물론 이러한 부분 또한 `Vue set()` API 등으로 해결할 수 있다.

### 4. 애플리케이션의 규모에 따라

위에서 말했듯이, Vue의 렌더링 시스템은 React보다 더 빠르다. 

또한, **웹 페이지의 용량 또한 Vue가 훨씬 가볍다.** 현재 릴리즈된 Vue 의 라이브러리 크기가 25.6KB 인 반면, 이 Vue 라이브러리와 동일한 기능을 하도록 React 의 부가 라이브러리로 구성한 React 라이브러리 (React DOM 37.4KB + React Addons Library 11.4KB) 의 크기는 48.8KB 이다. (2배)

따라서, **경량**의 앱을 제작해야 하는 경우에 Vue는 좋은 선택이 될 수 있다. 그렇다면 React는 어떨까?

React는 Javascript 기반의 문법을 Vue보다 더 적극적으로 사용하기 때문에, **JS로 구성된 컴포넌트는 재사용성이 높고, 테스트도 보다 수월하게 된다.**

또한 위에서 언급한 state의 불변성이 처음에는 복잡할 수도 있지만, **보다 복잡한 규모의 앱**에서는 테스트와 유지보수 측면에서 더 유리하다.

### 5. 개발 생태계

![](https://images.velog.io/images/ken1204/post/d5a7eebf-2f8e-4fa7-acab-a804357470ed/image.png)

2020년 기준으로 봤을 때, 위 그래프만 봐도 React가 압도적으로 **가장 인기있는 FE 라이브러리**라는 것은 의심의 여지가 없다. (2021년도 마찬가지다)

인기가 많다는 건, **더 많은 학습 자료와 개선, 혹은 Stack Overflow의 더 많은 질의응답이 있다는 것을 의미**한다. 이러한 생태계 부분은 개발자에게 있어서 React를 선택하게 된 가장 큰 이유가 될 정도로 중요하다. (나 또한 이 때문에 React를 선택했다)

또한 React는 Facebook에서 시작해서 지속적으로 관리되고 있다. 이런 측면에서 React를 사용하는 개발자나 기업 모두 React의 지속적인 버전업을 신뢰할 수 있게 된다.

Vue 또한 지속적으로 버전업을 하며, 신뢰성이 있는건 마찬가지지만 확실히 React에 비하면..? 아무래도 개발 생태계 부분은 확실히 미흡하긴 하다.

## 결론

그래서 결론은

> 기존에 이미 익숙한게 있다면 굳이 바꿀 필요는 없다. 쓰고 싶은거 쓰면 된다!



### 참고 링크
[React or Vue: Which JavaScript UI Library Should You Be Using?](https://dzone.com/articles/react-or-vue-which-javascript-ui-library-should-yo)
[React 인가 Vue 인가?](https://joshua1988.github.io/web_dev/vue-or-react/)
[다른 프레임워크와의 비교 - Vue.js](https://kr.vuejs.org/v2/guide/comparison.html#React)
[React.js vs Vue.js 자세한 비교 (1) - 공통점](https://velog.io/@vraimentres/react-vs-vue-1)