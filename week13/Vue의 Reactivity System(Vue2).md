 > Vue는 React와 달리, `setState()` 같이 상태를 set해주는 함수를 쓰지 않고도 data, 즉 상태를 변경하면 **자동**으로 상태가 업데이트 된다. 이게 어떻게 가능한걸까? 이게 궁금해서 이 글을 작성하게 되었다.

일반적인 블로그 글을 보면 Vue.js는 getters/setters를 이용해 이게 가능하다고 되어 있는데, 단순히 이렇게만 설명하면 어떤 식으로 동작해서 그렇게 되는지? 공식문서만 보고는 이해를 못해서 참고 [영상](https://www.vuemastery.com/courses/advanced-components/build-a-reactivity-system/)을 보고 이해한 내용을 바탕으로 작성해본다.

그 전에 Vue.js 공식문서를 먼저 보자.

## **변경 내용을 추적하는 방법**
아래 이미지 까지는 공식문서의 내용을 그대로 인용하였다.

Vue 인스턴스에 JavaScript 객체를 `data` 옵션으로 전달하면 Vue는 모든 속성에 **[Object.defineProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)**를 사용하여 **[getter/setters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects#Defining_getters_and_setters)**로 변환합니다. 이는 Vue가 ES5를 사용할 수 없는 IE8 이하를 지원하지 않는 이유입니다.

getter / setter 는 사용자에게는 보이지 않으나 속성에 액세스 하거나 수정할 때 Vue가 의존성 추적 및 변경 알림을 수행할 수 있습니다. 한가지 주의 사항은 변환된 데이터 객체가 기록될 때 브라우저가 getter / setter 형식을 다르게 처리하므로 친숙한 인터페이스를 사용하기 위해 **[vue-devtools](https://github.com/vuejs/vue-devtools)**를 설치하는 것이 좋습니다.

모든 컴포넌트 인스턴스에는 해당 **watcher** 인스턴스가 있으며, 이 인스턴스는 컴포넌트가 의존적으로 렌더링되는 동안 “수정”된 모든 속성을 기록합니다. 나중에 의존적인 setter가 트리거 되면 watcher에 알리고 컴포넌트가 다시 렌더링 됩니다.

![](https://images.velog.io/images/ken1204/post/88074472-6509-4de0-bd39-a877d6f7d0a8/image.png)

---

먼저, Vue가 reactivity system을 위해 getter/setter을 사용하는 이유까지 가기 위해서는 먼저 **의존성(dependency)**과 **watcher**이라는 개념에 대해 이해해야 한다.

이런 Javascript 코드가 있다고 해보자.

```jsx
		let price = 5
    let quantity = 2
    let total = price * quantity  // 10
    price = 20
    console.log(`total is ${total}`) // 10
```

맨 아래 콘솔에 찍히는 `total` 은 10이다. `price`가 20으로 바뀌었지만 당연히 결과는 10이다. 이걸 Vue가 상태를 업데이트 하는 방식처럼 Reactive 하게 바꿔보자.

### Problem

`total` 을 어딘가에 저장해야 `total`을 재실행하여 `price` 또는 `quantity`를 변경할 수 있다. 결국 `total`을 어딘가 저장하는 것이 관건.

### Solution

`price` 또는 `quantity` 변수가 업데이트 되면, 저장된 `total`을 다시 실행해야 한다. `total`을 저장하기 위한 `record()` 함수를 만들자.

```jsx
		let price = 5
    let quantity = 2
    let total = 0
    let target = null
    
    target = function () { 
      total = price * quantity
    }

	let storage = [] // target function을 저장할 배열
		
	price = 20    

    function record () {
      storage.push(target) // record 함수는 stoarge 배열에 target 함수를 저장한다.
    }
    
    record() // total을 저장하기
    target()
```

`storage` 배열에 저장된 `target`을 꺼내올 `replay()` 함수를 만들자.

```jsx
    function replay (){
      storage.forEach(run => run())
    }
```

```jsx
	let price = 5
    let quantity = 2
    let total = 0
    let target = null
    let storage = []
    
    function record () { 
      storage.push(target)
    }
    
    function replay () {
      storage.forEach(run => run())
    }
    
    target = () => { total = price * quantity }
    
    record()
    target()
    
    price = 20
    console.log(total) // => 10
    replay()
    console.log(total) // => 40
```

이제 전체 코드를 보자. `record()`를 통해 `total`을 저장하고, 저장한 `total`을 꺼내오기 위해 `replay()` 함수를 호출한다. 그럼 맨 아래 콘솔에는 변화된 price, 40이 찍히게 된다.

### 위 로직을 Class를 활용해 바꿔보자

위의 `record(), replay(), storage` 등은 모두 상태에 관련한 로직이므로 하나의 클래스로 묶어보자.

```jsx
	class Dep { // Stands for dependency
      constructor () {
        this.subscribers = [] //  storage 배열을 대체한다. 이게 흔히들 알고 있는 의존성 배열이다.
     	}
      depend() {  // record() 함수를 대체한다.
        if (target && !this.subscribers.includes(target)) {
          this.subscribers.push(target)
        } 
      }
      notify() {  // replay() 함수를 대체한다.
        this.subscribers.forEach(sub => sub())
      }
    }
```

그럼 이제 다시 콘솔에 price를 찍어보자. subscribers 배열을 의존성 배열이라고 말하겠다. 콘솔을 확인하면 정상적으로 출력되는 것을 확인할 수 있다.

```jsx
	const dep = new Dep()
    
    let price = 5
    let quantity = 2
    let total = 0
    let target = () => { total = price * quantity }
    dep.depend() // 의존성 배열에 현재 target 추가
    target()  // total을 얻기 위해 target 호출
    
    console.log(total) // => 10
    price = 20
    console.log(total) // => 10
    dep.notify()       // 배열 내부의 함수 실행
    console.log(total) // => 40 
```

### Watcher

앞으로 우리는 각 변수에 대해 `Dep` 클래스를 갖게 될 것이고,  업데이트를 감시해야 하는 임의의 함수를 생성하는 동작을 캡슐화하는 것이 좋을 것이다. 이를 위한 `watcher` 함수를 만들어보자.

```jsx
	// target, dep.depend()를 대신해서
	target = () => { total = price * quantity }
    dep.depend() 
    target()
```

```jsx
	// watcher를 함수(캡슐화를 위한)를 이렇게 만들어보고 싶다는 것이다. 
	watcher(() => {
      total = price * quantity
    })
```

그럼 이런 watcher 함수의 내부는 어떻게 구성해야 할까?

console에 찍었을 때 위와 같은 결과를 얻기 위해서는 watcher 함수 내부에 아래와 같이 작성해주자. 

```jsx
	function watcher(myFunc) {
	  target = myFunc    // 콜백 함수 myFunc
      dep.depend()       // 의존성 배열에 myFunc를 추가
      target()           // 받아온 함수 myFunc, 즉 target을 호출
      target = null      // Reset target
    }

	price = 20
    console.log(total)
    dep.notify()      
    console.log(total)
```

더 나아가서, 우리는 `notify()`도 따로 호출하지 않도록 각 변수가 `Dep` class를 가지고 있으면 좋겠다. 

![](https://images.velog.io/images/ken1204/post/43b91020-a9da-4bb6-993f-a2a9a08329a1/image.png)

- 각 변수가 `Dep` class를 가진다는 말을 그림으로 표현한 것

그걸 위해서 `data` 객체 안으로 `price` 속성과 `quantity` 속성을 줘보자.

```jsx
let data = { price: 5, quantity: 2 }
```

이렇게 바꾸면 위의 `watcher` 함수도 아래와 같이 바뀌게 된다.

```jsx
	watcher(() => {
      total = data.price * data.quantity
    })
```

이런 상황에서 새롭게 다른 `watcher` 함수를 하나 더 호출하는데, 이번에 호출된 `watcher`는 `data.quantity`는 건드리지 않고 `data.price` 만 건드리게 된다.

```jsx
	watcher(() => {
      salePrice = data.price * 0.9
    })
```

지금까지의 상황을 그림으로 요약하면 다음과 같다.

![](https://images.velog.io/images/ken1204/post/d547f989-878b-4f59-bc21-746c6f69fdcc/image.png)

- `data.price`를 감시하는 watcher 함수가 두 개가 있는게 보인다.

 

그럼 이제 우리는 어떤 `Dep` 인스턴스가 불려지는지 확인하려면 어떤 속성(`price, quantity`)이 각 `watcher` 내부에서 접근되어지는지를 식별해야 한다. 이를 그림으로 표현하면 다음과 같다.

![](https://images.velog.io/images/ken1204/post/a94b2c55-3c48-4ed1-8b75-f8a73d88e053/image.png)

 

그리고 언제 속성(`price, quantity`)이 **업데이트** 되는지도 확인해야 한다. 이를 확인하면 `dep.notify()` 를 언제 `Dep` 클래스로부터 호출해야 하는지 알 수 있다. 

**쉽게 얘기해서 이렇게 `dep.notify()`를 굳이 호출해줄 필요 없게 만들어주자는 말이다.**

![](https://images.velog.io/images/ken1204/post/dcf24edf-a4eb-4161-9d88-4113027c56b0/image.png)

 

이를 위해서 Vue.js에서는 `[Object.defineProperty()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)`를 사용하게 된다. 여기서 getter/setter의 개념이 나오게 된다. `Object.defineProperty()` 메서드의 세번째 매개변수인 `descriptor` 내부의 속성인 `get, set`을 정의해주는 것이다. 아래 코드를 보자.

```jsx
	let data = { price: 5, quantity: 2 }
    
    Object.defineProperty(data, 'price', {  // price 속성만
    
        get() {  // get 메서드 생성 
          console.log(`I was accessed`)
        },
        
        set(newVal) {  // set 메서드 생성
          console.log(`I was changed`)
        }
    })
    data.price // This calls get()
    data.price = 20  // This calls set(20)
```

이런 방식으로 `get, set`을 오버라이딩 하는 것이다. 개념을 알았으니 실제 위에서 작성하고 있던 코드에 적용시켜보자.

```jsx
	// 코드에 적용된 예시 (data.price만)
	let data = { price: 5, quantity: 2 }
    
    let internalValue = data.price // data.price를 저장하기 위한 변수 internalValue 
    
    Object.defineProperty(data, 'price', { 
    
        get() {  // Create a get method
          console.log(`Getting price: ${internalValue}`)
          return internalValue
        },
        
        set(newVal) { 
          console.log(`Setting price to: ${newVal}` )
          internalValue = newVal // 현재 value를 set 해준다.(바꿔준다)
        }
    })
    total = data.price * data.quantity  // This calls get() 
    data.price = 20  // This calls set()
```

그럼 이제 `data.price` 뿐만 아니라 `data.quantity`에도 적용시켜보자.

```jsx
	let data = { price: 5, quantity: 2 }
    
    Object.keys(data).forEach(key => { // data객체의 key를 순회하기 위해
      let internalValue = data[key]
      Object.defineProperty(data, key, {
        get() {
          console.log(`Getting ${key}: ${internalValue}`)
          return internalValue
        },
        set(newVal) {
          console.log(`Setting ${key} to: ${newVal}` )
          internalValue = newVal
        }
      })
    })
    total = data.price * data.quantity
    data.price = 20
```

이제 여기에 아까 만든 `watcher()` 함수를 결합시켜주면 Vue.js의 data reactivity system과 어느 정도 비슷하게 만들어지게 된다.

### 최종코드

```jsx
	let data = { price: 5, quantity: 2 }
    let target = null
    
    // 아까 봤던 Dep class이다.
    class Dep {
      constructor () {
        this.subscribers = [] 
      }
      depend() {  
        if (target && !this.subscribers.includes(target)) {
          this.subscribers.push(target)
        } 
      }
      notify() {
        this.subscribers.forEach(sub => sub())
      }
    }
    
    // Go through each of our data properties
    Object.keys(data).forEach(key => {
      let internalValue = data[key]
      
      // Each property gets a dependency instance
      const dep = new Dep()
      
      Object.defineProperty(data, key, {
        get() {
          dep.depend() // <-- Remember the target we're running
          return internalValue
        },
        set(newVal) {
          internalValue = newVal
          dep.notify() // <-- Re-run stored functions
        }
      })
    })
    
    // 더 이상 watcher 함수는 dep.depend()를 호출할 필요가 없게 된다. 
	// 위에서 get 메서드에서 호출 중이기 때문
    function watcher(myFunc) {
      target = myFunc
      target()
      target = null
    }
    
    watcher(() => {
      data.total = data.price * data.quantity
    })
```

## 결론
이건 Vue.js에서 작동하는 코드를 훨씬 간략화 시킨 것이다. 그러나 전체적인 틀은 같다고 한다.(Vue.js에서 서포트하는 영상이니까 맞겠지 아마?)

시간 남으면 [Build a Reactivity System](https://www.vuemastery.com/courses/advanced-components/build-a-reactivity-system/) 이 영상을 꼭 봐보자. 아주 훌륭하게 설명해주는데, 무료다.



## 참고 링크
- [Build a Reactivity System](https://www.vuemastery.com/courses/advanced-components/build-a-reactivity-system/)

- [반응형에 대해 깊이 알아보기](https://kr.vuejs.org/v2/guide/reactivity.html)

- [deep-dive-into-reactivity-in-depth](https://github.com/pistis/vuejs-deep-dive/blob/master/v2.5.21/deep-dive-into-reactivity-in-depth.md)