출처: 프론트엔드 3주차 수업 (Core Javascript 1)

# 동기(Synchronous)

현재 실행 중인 코드가 끝나야 다음 코드를 실행
= 현재 실행 중인 task가 종료할 때까지 다음 task는 대기
<img src="https://images.velog.io/images/yena1025/post/13788e4f-a984-4e52-bb7f-3c31012bf128/image.png" width="500" />

- 장점: 코드 실행 순서가 보장된다.
- 단점: 현재 실행 중인 task가 끝날 때까지 다음 task가 실행이 안 된다.
  (= task가 블로킹(blocking) 된다)

#### 동기 blocking 예제

```
const arr = [];

for (let i = 0; i < 1000; i++) {
  arr.push(i);
	// 복잡한 코드 (1000번 실행)
}

alert("안녕하세요");  // 확인 누를 때까지 console 코드로 넘어가지 않음
console.log(arr);
```

# 비동기(Asynchronous)

**현재 실행 중인 코드가 완료되지 않아도 **
나중에 실행하라고 브라우저에 맡겨놓고 **다음 코드로 넘어감**

```
// 코드 1

axios.get(url)
	.then(response => {
		// api 호출하고 응답받으면 실행
	})

// 코드 2
```

- api 호출하고 언제 응답이 올지는 모름

  1. 코드 1 하고
  2. **api 호출하고**
  3. 코드 2 하다가
  4. **응답오면 로직 실행하고**
  5. 다시 다음 로직 진행

- 장점: blocking이 발생하지 않는다.
- 단점: 코드 실행 순서가 보장되지 않는다.

## 비동기 처리가 필요한 이유

자바스크립트 엔진이 **싱글 스레드**이기 때문이다.

> **싱글 스레드 (Single Thread)**
> 호출 스택이 하나여서 한 번에 하나의 task만 실행할 수 있는 시스템

=> 처리에 시간이 걸리는 task를 실행하는 경우 Blocking(작업 중단)이 발생한다.

### 여기서 콜백함수 등장!

콜백함수로 비동기 작업을 처리할 수 있다.
주의: 콜백함수가 비동기에서만 쓰이는 것은 아니다. (동기에서도 쓰인다)

# 콜백함수(Callback Function)

- 함수 안에 인수로 전달된 함수

#### 고차 함수(Higher Order Function)

콜백함수를 가진 함수 (예: map)

> cf. 고차 컴포넌트 Higher Order Component(HOC)
> : 컴포넌트를 매개변수로 받아서 컴포넌트를 반환하는 컴포넌트

이렇게 함수의 인수로 함수를 전달할 수 있는 것은
자바스크립트가 일급함수를 가질 수 있는 함수형 프로그래밍 언어이기 때문이다.

#### 일급함수

> 함수가 변수로서의 특징을 가지는 것

    1. 함수를 다른 함수에 인수로 제공 가능
    2. 함수값 반환 가능
    3. 함수를 변수에 할당 가능

## 비동기 콜백함수 예시

### 이벤트 리스너 (addEventListener)

특정 이벤트 발생 시

### 타이머 (setTimeout, setInterval 등)

일정 시간 경과 후

### 통신 요청 (fetch, ajax, axios 등)

서버에 통신 요청 시

> **AJAX(Asynchronous JavaScript and XML)**
> 자바스크립트를 사용하여 브라우저가 서버에게
> **비동기 방식**으로 데이터를 요청하고, 응답한 데이터를 수신하여
> 웹페이지 일부를 **동적으로 갱신**하는 방식

# 콜백지옥 (Callback Hell)

api가 이렇게 오면 프론트엔드 입장에서 편하다. (호출 한번으로 필요한 데이터를 한꺼번에 뽑을 수 있기 때문)

```
[{
	"id": 1,
	"name": "운동화",
	"price": 30000
	"comments": [{
		"comment": "강추합니다",
		"username": "Kim",
		"likes": [{
			"like": true,
			"username": "lee"
		}]
	}]
}]
```

그런데 이렇게 3개의 api로 나뉘어 와 버렸다면?

```
[{
	"id": 1,
	"name": "운동화",
	"price": 30000
}]
```

```
[{
	"comment": "강추합니다",
	"username": "Kim"
}]
```

```
[{
	"like": true,
	"username": "lee"
}]
```

로직:

1. **상품 목록**을 가져온다.
2. 가져온 상품 목록 중에서 **첫 상품의 id**로 해당 상품의 후기 api를 호출한다.
3. 가져온 후기 중에서 **첫 후기의 id**로 좋아요 수를 가져오는 api를 호출한다.

코드를 짜면..

#### 콜백지옥 탄생..

```
$.get('https://api.test.com/proudcts', function(response) {
	var firstProductId = response.products[0].id;

	$.get('https://api.test.com/proudct/comments?id='+firstProductId, function(response) {
		var firstCommentId = response.comments[0].id;

		$.get('https://api.test.com/proudct/comment/likes?id='+firstCommentId, function(response) {
			var likes = response.likes;
			var likesCount = likes.length;
		});
	});
});
```

# Promise

- Promise는 이러한 **콜백 지옥을 해결**하고 **비동기 동작을 처리**하기 위해
  **ES6에 도입**되었다.
- Promise는 클래스이다. -> Promise 클래스를 인스턴스화 해서 promise 객체를 만든 뒤(`let promise = new Promise()`) 반환된 promise로 원하는 비동기 동작을 처리한다.

## Promise 구현

```
let promise = new Promise(function(resolve, reject) {
	// 비동기(발동 후 언제 완료될지 모르는) 로직 작성
});
```

- **resolve**는 비동기 로직이 **성공**했을 때 실행할 함수
- **reject**는 비동기 로직이 **실패**했을 때 실행할 함수

## resolve

```
let promise = new Promise(function(resolve, reject) {
	// 비동기 로직
	// 성공 하면 -> resolve 호출
  	// 실패 하면 -> reject 호출
});

// resolve 함수
promise.then(function() {
	// 위의 비동기 로직이 성공하면 실행
});
```

### 예제

```jsx
let promise = new Promise(function(resolve, reject) {
    setTimeout(function() {
		// resolve 함수에 인자 넘기면
        resolve('hello world');
    }, 2000);
});

promise.then(function(msg) {
    console.log(msg);  // 2초 뒤에 실행 (hello world)
}**);
```

## reject

```jsx
let promise = new Promise(function (resolve, reject) {
  setTimeout(function () {
    reject('으악!');
  }, 2000);
});

promise.then(
  function () {
    console.log('resolve'); // 여기는 실행이 안 되고
  },
  function (msg) {
    console.log('reject', msg); // 여기만 실행됨 "reject 으악!"
  }
);
```

#### then으로 두 개 받는 것보다는 catch가 가독성이 더 좋다.

```
let promise = new Promise(function(resolve, reject) {
	setTimeout(function() {
        reject('으악!');
  }, 2000);
});

promise
	.then(function() {})
	.catch(function(err) {
		console.log(err);
	});
```

### 주의할 점

```jsx
let promise = new Promise(function (resolve, reject) {
  setTimeout(function () {
    resolve(1);
    resolve(2);
  }, 1000);
});

promise.then(function (msg) {
  console.log(msg);
});
```

- 첫 번째 resolve만 실행됨 (reject도 마찬가지)

# Quiz

답은 바로 F12 눌러 console 창에 복붙하신 후 엔터 치면 알 수 있다.
(하지만 먼저 풀어보고 확인하기!)

### 문제 1

```jsx
let promise = new Promise(function (resolve, reject) {
  setTimeout(function () {
    console.log(1);
    resolve(2);
    console.log(3);
    resolve(4);
  }, 1000);
});

promise.then(function (data) {
  console.log(data);
});
```

### 문제 2

```jsx
let promise = new Promise(function (resolve, reject) {
  resolve(1);

  setTimeout(function () {
    resolve(2);
  }, 1000);
});

promise.then(function (data) {
  console.log(data);
});
```

### 문제 3

중간에 catch 있는 거 주의

```
function job() {
    return new Promise(function(resolve, reject) {
        reject();
    });
}

job()
	.then(function() {
    console.log(1);
	})
	.then(function() {
    console.log(2);
	})
	.then(function() {
    console.log(3);
	})
	.catch(function() {
    console.log(4);
	})
	.then(function() {
    console.log(5);
	});
```

# promise의 3가지 상태

- promise 객체는 3가지 상태(status)를 갖고 있다.
  - **Pending(대기)**: 비동기 처리 로직이 아직 완료되지 않은 상태
  - **Fulfilled(이행)**: 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
  - **Rejected(실패)**: 비동기 처리가 실패하거나 오류가 발생한 상태

## Quiz

### 문제 1

```
let promise = new Promise(function(resolve, reject) {
    setTimeout(function() {
      console.log('before', promise)
      resolve(1);
      console.log('after', promise);
    }, 1000);
});

promise.then(function(data) {
    console.log(data);
});

```

### 문제 2

```
let promise = new Promise(function(resolve, reject) {
    setTimeout(function() {
      console.log('before', promise)
      reject(1);
      console.log('after', promise);
    }, 1000);
});

promise.then(function(data) {
    console.log('resolve', data);
}, function (data) {
    console.log('reject', data);
});
```

# 추가 내용

## Promise.all()

- 여러 프로미스를 **모두 성공시킨 후**, 완료 로직을 실행하고 싶은 경우
- resolve가 여러 개여도 모두 실행시킬 수 있다.
  ```jsx
  Promise.all([
    new Promise((resolve) => setTimeout(() => resolve(1), 9000)), // 1
    new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
    new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
  ]).then(function (value) {
    console.log(value);
  });
  ```
  - 결과: 9초 뒤에 value의 값이 `[1, 2, 3]` 으로 들어온다.

#### 자주 사용하는 예시

    let urls = [
      'https://api.github.com/users/iliakan',
      'https://api.github.com/users/remy',
      'https://api.github.com/users/jeresig'
    ];

    // map every url to the promise of the fetch
    let requests = urls.map(url => fetch(url));

    // Promise.all waits until all jobs are resolved
    Promise
    	.all(requests)
      .then(responses => responses.forEach(
        response => alert(`${response.url}: ${response.status}`)
      ));

# async & await

- async function은 ES8에 도입되었으며, 비동기 함수를 선언한다.
- async function은 promise를 반환하기 때문에 Promise를 잘 알아야 한다.
- **async 함수 안에서만 await를 쓸 수 있다.**

## 즉시 실행함수(Immediately invoked function expression, IIFE)

### 형태

```
(async function() {
	// 내용
})();

(async () => {
	// 내용
})();
```

### 예시

```
axios('https://api.test.com/proudcts')
  .then(function(response) {
    let firstProductId = response.products[0].id;
		return axios('https://api.test.com/proudct/comments?id='+firstProductId);
  })
  .then(function(response) {
		let firstCommentId = response.comments[0].id;
    return axios('https://api.test.com/proudct/comment/likes?id='+firstCommentId)
  })
	.then(function(response) {
			let likes = response.likes;
			let likesCount = likes.length;
  });
```

위의 코드를 **async/await + 즉시실행함수 + 예외처리** 로 바꾸면

```
(async () => {
	try {
	  let productResponse = await fetch('https://api.test.com/proudcts');
	  let firstProductId = productResponse.products[0].id;

	  let commentResponse = await fetch('https://api.test.com/proudct/comments?id='+firstProductId);
		let firstCommentId = commentResponse.comments[0].id;

	  let likeResponse = await fetch('https://api.test.com/proudct/comment/likes?id='+firstCommentId);
		let likesCount = likeResponse.likes.length;
	} catch(error) {
		console.log(error);
	}
})();
```

# 이벤트루프

이벤트루프에 대한 자세한 사항은 이전 포스트를 참고해주세요. 😊

이전 포스트: 여기로 이동 👉https://velog.io/@yena1025/%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84%EB%9E%80

![](https://images.velog.io/images/yena1025/post/ec058ec8-ce2c-4de2-9ece-064847bba38f/image.png)

## Quiz

```
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"), 500);
const baz = () => console.log("Third");

bar();
foo();
baz();
```

- 이벤트 루프에서 어떻게 작동할지 펜으로 그려보고

* 실행결과 예측해보기
