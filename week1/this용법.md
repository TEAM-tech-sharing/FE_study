# this 용법

출처: [https://nykim.work/71](https://nykim.work/71)

자바스크립트에서 **this**는 '**누가 나를 불렀는가**'를 의미

즉, **선언이 아닌 '호출'에 따라 달라지는 것**

\*\* '바인딩' = 가리킨다!

### this가 바인딩되는 5가지 상황

1. **단독으로** 사용
2. **함수** 안에서 사용
3. **메서드** 안에서 사용
4. **이벤트 핸들**러 안에서 사용
5. **생성자** 안에서 사용

## **1. 단독으로 쓴 this**

**그냥 this를 호출**하면 global object**(= window)**를 가리킴 **(strict mode에서도 마찬가지)**

(브라우저 콘솔에서 호출하는 경우 [object Window])

```python
var **x** = **this**;
console.log(**x**); // **window**
```

## **2. 함수 안에서 쓴 this**

**함수 안에서 this**는 함수의 주인 \***\*객체**(= window)\*\*에게 바인딩 (⇒ default 값)

```python
**function** **myFunction()** {
  return **this**;
}
console.log(**myFunction()**); // **window (함수 안에서 선언된 this는 window)**
```

```jsx
var num = 0;

**function addNum()** {
  **this.num** = 100; // **this = window (this.num = 전역에 선언된 num)**
  **num++**;

  console.log(**num**); // 101
  console.log(**window.num**); // 101
  console.log(**num === window.num**); // true
}

addNum();
```

\*\* **strict mode**에서는 **함수 내부 this에 default 값(window)이 없다고 간주**해서 그냥 **undefined** 출력

```python
"use strict";

**function** **myFunction()** {
  return **this**;
}

console.log(**myFunction()**); // **undefined**
```

```python
"use strict";

var num = 0;

**function addNum()** {
  **this.num** = 100; // '**ERROR! Cannot set property 'num' of undefined(= 함수 안의 this)'**
  num++;
}

**addNum()**;
```

## **3. 메서드 안에서 쓴 this**

일반 함수가 아닌 **메서드(= 객체 안에서 선언된 함수)**라면?

메서드 내부에서 사용된 this는 **해당 메서드를 호출한 객체로 바인딩**됩니다.

```python
var **person** = {
  firstName: 'Yena',
  lastName: 'Yun',
	**fullName**: **function** () {
    return **this**.firstName + ' ' + **this**.lastName; // **this는 person 객체**
  },
};

person.**fullName()**; // "Yena Yun"
```

### 일반 함수와 메서드 안의 함수의 차이

```python
var **num** = 0;

**function showNum()** {
  console.log(**this.num**); // **this = window**
}

**showNum(**); // 0

var obj = {
  num: 200,
  **func**: **showNum**, // 여기서 showNum 안에 있는 this는 obj 객체를 가리킴 (this.num = obj.num)
};

**obj.func()**; // 200
```

## **4. 이벤트 핸들러 안에서 쓴 this**

**이벤트 핸들러에서 this**는 **이벤트가 발생하는 HTML 요소**를 가리킴

```python
var **btn** = document.querySelector('**#btn**')
**btn**.addEventListener('click', function () {
  console.log(**this**); // **this는 btn**
});
```

## **5. 생성자 안에서 쓴 this**

**생성자 안에서의 this**는 **생성자 함수가 생성(new)하는 객체**로 바인딩

```python
function Person(**name**) {
  **this.name** = **name**; // **this는 생성자 함수로 생성된 kim, lee**
}

// kim, lee = new 키워드와 생성자 함수(Person)로 생성된 객체
var **kim** = **new** Person('kim');
var **lee** = **new** Person('lee');

console.log(**kim.name**); // kim 객체의 name = 생성자 함수 인자로 들어간 'kim'
console.log(**lee.name**);
```

** 주의: **new\*\* 키워드가 없으면 일반 함수 호출과 같아짐 ⇒ this가 window에 바인딩

```python
var name = 'window';

function **Person**(**name**) {
  **this.name** = **name**; // **this는 window!**
}

var **kim** = **Person**('kim'); // **new가 없어서 Person은 일반 함수!**

console.log(**window.name**); // kim
```

## 6. 명시적 바인딩을 한 this (apply, call 메서드)

**명시적 바인딩:** 짝을 지어주는 것 (이 this는 내꺼! 같은 느낌)

apply( )와 call( ): \*\*\*\*함수 객체(Function Object)에 기본적으로 정의된 메서드

**(메서드에 들어온 인자를 this로 만들어 줌)**

```jsx
function **whoisThis**() {
  console.log(**this**);
}

whoisThis(); // window (그냥 호출하면 함수 내부의 this는 window)

// 객체 생성
var **obj** = {
  x: 123,
};

// **함수에 call 메서드를 실행**하고 **인자에 객체**를 넣어줌 (=> 객체가 함수 내부의 this가 됨)
**whoisThis**.**call**(**obj**); // {x: 123}
```

### apply( )

**1번째 인자**: apply 메서드를 호출하는 함수

**2번째 인자(배열)**: 호출한 함수에서 바인딩할 요소

\*\* **apply는 배열에 담고 call은 그냥 ' , '로만 연결**한다는 차이가 있음

두 함수의 특정 부분이 똑같을 때 apply()나 call()을 사용하여 중복 코드를 줄임

(약간 상속? 해서 쓰는 느낌)

**두 함수 생성**

```jsx
function Character(name, level) {
  **this.name = name;
  this.level = level;**
}

function Player(name, level, job) {
  **this.name = name;
  this.level = level;**
  this.job = job;
}
```

**apply 적용**

```jsx
function Character(name, level) {
  this.name = name;
  this.level = level;
}

function **Player**(name, level, job) {
	// apply 적용
  **Character.apply(this, [name, level]);**
  this.job = job;
}

// 사용 (새로운 객체 생성)
var me = new **Player**('Nana', 10, 'Magician');
```

**call 적용**

```jsx
function Character(name, level) {
  this.name = name;
  this.level = level;
}

function Player(name, level, job) {
  **Character.call(this, name, level);**
  this.job = job;
}

// 사용 (빈 객체 생성 후 call 인자에 추가해서 사용)
**var me = {};
Player.call(me, 'nana', 10, 'Magician'); //** call(**{}**, name, level, job)
```

## 7. **화살표 함수로 쓴 this**

**왜 함수 안에서 this가 전역 객체가 되는 거야** 싶으면 화살표 함수 사용

화살표 함수는 전역 context(문맥)에서 실행되더라도 바로 바깥 함수나 클래스의 this를 사용

**내부 함수에서 this가 전역 객체를 가리키는 바람에 의도와는 다른 결과가 나온 경우**

```python
var **Person** = **function** **(name, age)** {
  this.name = name;
  this.age = age;

  **this.say** = **function** () {
		// 메서드의 this는 메서드를 호출한 객체(Person)에 바인딩
    console.log(this); // Person {name: "Nana", age: 28} (인자가 들어간 것까지만 나옴)

		**// setTimeout의 콜백함수**로 **일반 함수**를 넣음
    **setTimeout**(**function ()** {
      console.log(**this**); // 일반 함수 this는 **window(전역객체)가 됨

			// window.name, window.age가 되어서 아래와 같이 이상한 결과 나옴**
      console.log**(this.name** + ' is ' + **this.age** + ' years old');
    }, 100);
  };
};

var **me** = **new Person('Nana', 28)**; // Person 객체에 name, age 인자를 넣음

**me.say**(); // **global is undefined years old**
```

**this를 실제 사용하는 곳을 화살표 함수로 바꾸면 제대로 된 결과가 나옴!**

```python
var Person = function (name, age) {
  this.name = name;
  this.age = age;

  this.say = function () {
    console.log(this); // Person {name: "Nana", age: 28}

		**// setTimeout의 콜백함수**로 **화살표 함수**를 넣음
    **setTimeout(() => {
			//** **화살표함수 내부의 this는 전역객체로 바뀌지 않음
      // this = 메서드를 호출한 객체(Person)**
			console.log(this); // Person {name: "Nana", age: 28}
			// **this.name = 'Nana', this.age = 28**
      console.log**(this.name** + ' is ' + **this.age** + ' years old');
    **}**, 100);
  };
};

var me = new Person('Nana', 28);

**me.say**(); // **Nana** is **28** years old
```
