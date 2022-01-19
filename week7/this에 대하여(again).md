# This란?

출처: [10분 테코톡 - 브콜의 This] https://www.youtube.com/watch?v=7RiMu2DQrb4

일반적으로 객체지향 언어에서의 **this**는
함수가 속해 있는 객체의 **자기 자신**과 굉장히 관련이 깊다.

자바스크립트에서는 자기 자신이라는 말이 상당히 모호한데,
그 이유는 자바스크립트의 함수가 **'일급 객체'**이기 때문이다.

### 일급 객체의 특징

<img src="https://images.velog.io/images/yena1025/post/dfdc4fc5-62bc-4816-a904-43efa9216460/image.png" width="350"/>

자바스크립트의 **함수**는 **일급 객체**이다.

일급 객체는
**변수에 저장**될 수 있으며,
**함수의 인수로 전달**이 가능하고
**함수의 반환값**으로 사용할 수 있다.

<img src="https://images.velog.io/images/yena1025/post/2529e697-b8ca-467e-8da8-c4eafb3ba92a/image.png" width="500" />

사용 시에는 단독으로 호출되거나
특정 객체의 '메서드'로,
또는 동일한 메서드를 다른 객체에서 호출하는 것도 가능하다.

(Q. 그 객체에 정의되어 있어야 호출이 가능한 것 아닌지?
// '이름'만 동일한 메서드?)
<br/>

이렇게 자바스크립트에서는 **함수가 선언된 이후
어떤 환경에서 어떤 객체에 의해 호출될지**에 따라 this가 달라진다.

> 예: 브로콜리가 "내 꺼"라고 외치고 아보카도가 "내 꺼"라고 외쳤을 때
> '나'가 가리키는 대상은 각각 다르다.

### 바인딩 이란?

자바스크립트에서 모든 함수는 this를 가지고 있으며,
함수가 호출되면 그때그때 상황에 따라 this가 가리키는 객체가 결정된다.

이렇게 **함수가 호출될 때마다
this가 가리키는 객체가 동적으로 결정되는 것**을
this가 그 객체에 **'바인딩'** 된다고 표현한다.

### this가 바인딩되는 상황

<img src="https://images.velog.io/images/yena1025/post/73d511bf-4dbf-4d6c-8355-ce4b1cd780ef/image.png" width="500"/>

프로그램이 실행되면

1. 전역 코드를 평가해서 전역 실행 문맥을 만들고,
2. 함수가 실행되면 전역 코드의 실행을 잠깐 멈추고
   해당 함수의 실행 문맥을 만든다.
   이 때 이 함수의 실행 문맥에서 this 바인딩 컴포넌트의 값이 결정된다.

<br/>

# This의 바인딩 규칙

this는 기본적으로 4가지 규칙에 의해 바인딩 된다.

1. 기본 바인딩
2. 암시적 바인딩
3. new 바인딩
4. 명시적 바인딩

## 기본 바인딩

함수를 **단독 실행**할 때의 규칙이다.
브라우저 환경과 Node 환경의 두 가지 경우로 나뉜다.

### 브라우저 환경

<img src="https://images.velog.io/images/yena1025/post/b85db54b-c12f-4c3a-92a8-953cdb442cb1/image.png" width="600" />

브라우저 환경에서 **기본** 바인딩 대상은 **window** 전역 객체이다.

그러나 **use strict** 키워드('엄격 모드')를 사용할 경우에는
전역 객체가 기본 바인딩 대상에서 제외되어 버려서
this가 바인딩될 객체가 존재하지 않는 상태라 **undefined** 값을 가진다.

### Node.js 환경

<img src="https://images.velog.io/images/yena1025/post/b3afa21d-12a0-45be-ab09-cbe9df6d7eaa/image.png" width="600" />

Node.js 의 **기본** 바인딩 대상은 전역 객체 **global**이다.
다만 **함수 내부 코드**에서는 전역객체 global에 바인딩되지만
**전역**에서는 **빈 객체(= module.exports)**에 바인딩된다.

> module.exports란?
> 노드에서 파일을 모듈로 사용할 수 있게 해주는 객체이다.
> 노드에서 this, exports, module.exports는 모두 같다.

```
console.log(this === module.exports, this === exports); // true, true

function a() {
  console.log(this === exports); // 함수 안에서 출력하면 false, 전역에서는 true
}
```

<br/>

## 암시적 바인딩

<img src="https://images.velog.io/images/yena1025/post/2d3cee6d-b6b8-4bc5-930c-b5f4d2d888de/image.png" width="600" />

암시적 바인딩은 자바스크립트의 함수가
**객체의 메서드**로 호출이 되는 경우이다.
형태적으로는 **this가 점(.) 바로 앞에 있는 객체에 바인딩** 된다.

> **유의할 점**
> 이렇게 점(.)을 이용해 this를 바인딩하면
> 해당 메서드를 **함수의 인자로** 넣어 사용 시
> 기존 객체에 바인딩된 this가 소실(-> undefined)된다.

(참조 관련한 더 깊은 내용은 이후 공부할 예정)

## 명시적 바인딩

명시적 바인딩이란 **this를 '내가 정한 객체'로 고정**하여 바인딩하는 것이다.

이를 통해 암시적 바인딩에서의 '한두 다리만 건너도 this가 소실'되는 문제를 해결할 수 있다.

종류는 call, apply, bind 메서드가 있는데
셋이 형태나 실행 여부 등에서 약간씩의 차이만 있다.

#### call, apply 형태

<img src="https://images.velog.io/images/yena1025/post/e6e0806b-a106-4e86-bc71-bd03deb9a969/image.png" width="300" />

#### bind 형태

<img src="https://images.velog.io/images/yena1025/post/37c34aa2-8109-4306-a5cb-d0afd884a750/image.png" width="350" />

context 뒤에 오는 요소들은
**각 메서드의 앞에 오는 함수의 인수**들이며
context는 필수이고 뒤에 오는 인수들은 옵션이다.

<br/>

(아래 내용은 다른 블로그에서 참조하였습니다)
출처: https://kamang-it.tistory.com/entry/JavaScript07this-this%EB%B0%94%EC%9D%B8%EB%93%9C%ED%8E%B8bindcallapply

### bind

```
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    info(){
        console.log(`x: ${this.x}, y: ${this.y}`);
    }
}

var point = new Point(10, 20);

point.info();

point.info.bind({x: 100, y: 200})(); // 뒤에 소괄호를 붙여 실행!
```

<img src="https://images.velog.io/images/yena1025/post/040cf3d4-a290-4289-a6f7-65bcc8d576b7/image.png" width="300" />

**이미 생성된 Point 클래스(point)에 다른 인수를 넣어
수정해서 사용**하고 싶다면
새로운 클래스 객체를 또 만드는 게 아니라 bind 메서드를 활용하면 된다.
이때 bind 안에는 **객체{ } 형태**로 수정할 인수값들을 넣어준다.

**주의할 점**
bind는 반드시 뒤에 **소괄호 ( )**를 붙여줘야 **실행**이 된다.

### call, apply

```
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    info(v, w) {
        console.log(`x: ${this.x}, y: ${this.y}, v: ${v}, w: ${w}`);
    }
}

var point = new Point(10, 20);

point.info(1, 2);

var customPoint = {x: 100, y: 200};

point.info.call(customPoint, 20, 30);
point.info.apply(customPoint, [2, 3]);
```

<img src="https://images.velog.io/images/yena1025/post/e1cab1af-fbed-420c-b32a-dbedbe228875/image.png" width="250" />

call, apply는 **자체 실행**까지 한다.

bind는 연결만 하는 반면 call과 apply는 연결하고 실행까지 하는 것이다.

call과 apply 둘의 차이는
**call**은 함수를 실행할 때 인수를 **하나씩** 넘기고
**apply**는 **배열**로 넘긴다는 형태적 차이이다.

## new 바인딩

new 연산자로 호출할 때 this가 바인딩되는 규칙이다.

<img src="https://images.velog.io/images/yena1025/post/40e7e36a-691b-4f62-abc7-79883af0cec0/image.png" width="550" />

위의 과정에서 this는
1번째 단계인 **'새로운 객체 생성'** 단계에서 바인딩된다.

코드로 아주 간단하게 표현하면 다음과 같다.

<img src="https://images.velog.io/images/yena1025/post/56891828-577b-47cb-a397-904598b1021d/image.png" width="600"/>

이런 과정을 통해 property("beuccol")가 정해진 객체를 반환하는
**생성자로서의 역할**을 수행할 수 있게 된다.

### 추가

코드를 짜다보면 위와 같은 4가지 규칙 여러 개가
동시에 적용되는 상황도 발생하는데, 이 때의 우선순위는 다음과 같다.

<img src="https://images.velog.io/images/yena1025/post/48d33494-bf77-4e1b-be1f-0f39285046eb/image.png" width="600"/>

<br/>

# 화살표 함수

ES6 문법부터 새롭게 추가된 화살표 함수는
기존의 함수와 다른 방식으로 this를 바인딩한다.

<img src="https://images.velog.io/images/yena1025/post/8c89026e-ff92-4107-905b-f42c9326367d/image.png" width="600" />

위의 예제는 비동기 함수 setTimeout에 화살표 함수를 사용하고 있다.

화살표 함수의 가장 큰 특징 중 하나는
**선언될 당시의 '상위 실행 문맥을 유지'**하는 것이다.

이러한 특징 때문에 점(.)으로 호출되는 암시적 바인딩 형태에서도
**화살표 함수를 사용하면 this에 바인딩된 내용을 잃어버리지 않는다.**

이렇게 바인딩된 this를 '**어휘적 this**' 또는 '**렉시컬 this**'라고 부른다.

## 정리

자바스크립트 this의 이러한 특징들을 잘 이해하고
적절하게 사용한다면 유연한 프로그래밍에 도움이 될 것이다.
