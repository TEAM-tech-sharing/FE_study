> 이 글은 [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176) 을 읽고 이해한 부분을 요약, 정리한 글입니다. 모든 이미지, 내용 출처는 [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)와 [MDN Web Docs](https://developer.mozilla.org/ko/)를 참고하였습니다.

프론트엔드 개발자라면 `display: flex;` 속성은 정말 많이 사용하고 있을 것이다. 레이아웃을 쉽게 할 수 있는 방법이니까. 가로축 중앙 정렬을 위해서

```css
display: flex;
justify-content: center;
align-items: center;
```

아래와 같은 속성을 많이 사용하고 있을 것이다. 그럼 Flexbox가 정확히 어떻게 구성되는지 알아보자.

## Flexbox의 구성

![](https://images.velog.io/images/ken1204/post/620adfa5-01ad-481a-96b5-6d0a6da74a25/image.png)

- 출처: [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)

부모 요소가 `flex-container`, 자식 요소를 `flex-item`이라고 한다. 각각에 해당하는 속성은 다음과 같다.

- **flex container** 속성: `flex-direction, flex-wrap, justify-content, align-items, align-content`
- **flex item** 속성: `flex, flex-grow, flex-shrink, flex-basis, order`

![](https://images.velog.io/images/ken1204/post/55b30a0d-f11f-4383-9e2f-2ce988691995/image.png)

- 출처: [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)

### `flex-grow, flex-shrink, flex-basis`

아까 말했듯이, 부모는 `flex-container`, 자식은 `flex-item`으로 명명하겠다. 그럼 다음과 같은 코드를 잠시 보자.

```css
.flex_container {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.flex_item {
  flex: 1; /* flex: 1 1 0 */
  overflow: auto;
}
```

부모 코드는 대부분 알 거라고 생각한다. 그렇다면 아래 속성에서 `flex: 1`은 대체 뭘 의미하는걸까? 다음 그림을 보자.

![](https://images.velog.io/images/ken1204/post/815560a0-8a2b-4433-a994-ffa3ce4ab069/image.png)

- 출처: [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)

사실 `flex:1` 속성은 뒤의 두 속성, `flex-shrink`와 `flex-basis`가 생략된 속성이다. `flex` 속성은 다음과 같이 3가지로 이루어지는데,

### `flex-grow`

`flex-grow` 속성은 `flex item`의 **확장**에 관련된 속성이다.

0과 양의 정수를 속성값에 사용한다. 속성값이 0이면 `flex container`의 크기가 커져도 `flex item`의 **크기가 커지지 않고 원래 크기로 유지된다.** `flex container`의 크기가 커질 때 `flex item`의 크기도 커지게 하려면 1 이상의 값을 속성값으로 설정한다. 속성값이 1 이상이면 `flex item`의 원래 크기에 상관없이 `flex container`를 채우도록 `flex item`의 크기가 커진다.

![](https://images.velog.io/images/ken1204/post/0e5df883-24da-4e83-a74e-2e227edd0070/image.png)

- 실제 예제: [codesandbox: flex-grow](https://codesandbox.io/s/fragrant-microservice-0tk0l?file=/index.html:426-444)

### `flex-shrink`

flex-shrink 속성은 flex item의 축소에 관련된 속성이다. 0과 양의 정수를 속성값에 사용한다. 기본값은 1이다.

속성값이 0이면 `flex container`의 크기가 `flex item`의 크기보다 작아져도 `flex item`의 크기가 줄어들지 않고 원래 크기로 유지된다. 속성값이 1 이상이면 `flex container`의 크기가 `flex item`의 크기보다 작아질 때 `flex item`의 크기가 `flex container`의 크기에 맞추어 줄어든다.

![](https://images.velog.io/images/ken1204/post/b14ea216-421c-4f24-851e-2c8b8ddcf2c9/image.png)

### `flex-basis`

`flex-basis` 속성은 `flex item`의 기본 크기를 결정하는 속성이다. 기본값은 `auto`다.

![](https://images.velog.io/images/ken1204/post/bb410c55-0f50-4245-a009-09703dd43312/image.png)

`flex-basis` 속성의 값을 `auto`로 설정하면 `flex item`은 상대적 flex item(relative flex item)이 되어 콘텐츠의 크기를 기준으로 크기가 결정된다.

![](https://images.velog.io/images/ken1204/post/f87b8643-ef92-4e16-aa94-08c351a0a0c3/image.png

### `flex: 1`의 의미

`flex-grow` 속성과 `flex-shrink` 속성, `flex-basis` 속성을 축약해서 `flex` 속성으로 표현할 때 `flex: 1` 속성은 `flex: 1 1 0` 속성을 의미한다.

즉, `flex-grow` 속성의 값이 '1'이고 `flex-shrink` 속성의 값이 '1'이기 때문에 `flex container`의 크기에 따라 flex item의 크기도 커지거나 작아진다는 의미다.
![](https://images.velog.io/images/ken1204/post/a7effe01-f01e-4663-841d-2422b9824f9f/image.png)

`flex` 속성의 값으로 정수 하나만 선언하면 선언한 값은 `flex-grow` 속성의 값이 된다. 나머지는 기본값인 `flex-shrink: 1` 속성과 `flex-basis: 0` 속성이 적용된다. 다시 말해 `flex` 속성에 한 개의 정숫값만 있으면 이것은 `flex-grow` 속성의 값만 지정하는 단축 속성이다. 즉, `flex: 2`는 `flex: 2 1 0`을, `flex: 3`은 `flex: 3 1 0`을 나타내다.

### `flex` 속성에 사용되는 키워드에 따른 설정값

![](https://images.velog.io/images/ken1204/post/ba21275d-4887-4b1e-9657-ce37e3a7f1ce/image.png)

- 출처: [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)

## 내가 몰랐던 속성

### `flex: none`

그렇다면, `flex: none`이 속성이 되었을 때, `flex-item` 박스는 어떻게 변할까?
![](https://images.velog.io/images/ken1204/post/eca7dc27-49fc-49d9-a5d0-6050da2771d8/image.png)

다음과 같이 `flex-item`의 크기가 고정된다.

아래 이미지는 `flex: 1`을 했을 때의 결과다. 각 `flex-item`이 부모 요소인 `flex-container`을 꽉 채우는 것을 볼 수 있다.
![](https://images.velog.io/images/ken1204/post/f98ccc9f-c43d-4e7b-8292-0a1eb5e31325/image.png)

### `margin-left: auto`

`margin-left: auto`는 Flexbox에서 요소를 오른쪽 정렬시킬 때 많이 쓰인다. 다음의 그림을 보면 이해가 쉬울 것이다.

![](https://images.velog.io/images/ken1204/post/de19101b-4289-47cc-a97f-0ff6c5e824f4/image.png)

- 출처: [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)

마찬가지로, 위의 그림과 같이 `margin-right: auto`는 왼쪽 정렬을 해준다. `margin: 0 auto`는 가운데 정렬을 해주지만, 이보다는 `justify-content: center`를 더 많이 사용하고, 직관적이니 굳이 언급하지 않겠다.

아래 이미지와 같이`flex-item`을 수직으로 배치할 때도 같은 방법을 적용할 수 있다.
![](https://images.velog.io/images/ken1204/post/680f90b9-386c-4d7a-a014-43527817ce65/image.png)

> `margin-top: auto`를 사용하면 브라우저 화면 아래에 딱 붙는 Footer 레이아웃을 잡을 때 사용하기 좋다!

flexbox를 사용하면 다음 그림과 같이 **콘텐츠의 길이와 상관없이 항상 화면 아래에 표시되는 푸터를 만들 수 있다.**

### 유동 너비 박스 만드는 방법

Flexbox를 자주 사용하던 FE 개발자라면 익숙한 개념일 것이다.

```css
.flex_container {
  display: flex;
}

.flex_item {
  max-width: 300px;
}
```

위와 같이 `max-width: 300px`만 설정해주면 박스의 크기는 `300px` 보다 크지 않는 선에서 유동적으로 변하게 된다.

![](https://images.velog.io/images/ken1204/post/6f51ead8-17a7-4432-9b91-5e8f550c4886/image.png)

- 출처: [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)

### `display: inline-flex`

난 지금까지 `display: flex`는 잘 사용해왔다. 그렇지만 `inline-flex`는 사용해본적이 없었다. 이 속성의 의미는 Flexbox를 이해하는데 큰 도움을 받았고, 지금 글을 작성하는데도 카피하다시피 참고하는.. [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176) 글의 내용을 인용했다.

> `display: inline-flex` 속성은 flex container를 **인라인 블록 요소**로 만드는 속성이다. `display: flex` 속성을 적용한 flex container는 **블록 요소**다. **블록 요소는 브라우저 화면이 한 줄 전체를 차지하지만, 인라인 블록 요소는 한 줄의 일부 영역만 차지한다.**

대체 무슨 소리인가 싶다. 다음 그림을 보자.

![](https://images.velog.io/images/ken1204/post/d51f9a2d-2557-4267-b684-840626c72cf1/image.png)

```css
.flex_container {
  display: inline-flex;
  max-width: 100%;
}
```

### `flex-wrap`

`flex-wrap`은 `flex item`이 `flex container`를 벗어났을 때 **줄을 바꾸는 속성**이다. `white-space` 속성과 동일하게 작동한다. 속성의 기본값인 `nowrap`은 `flex item`의 전체 크기가 `flex container`보다 커져도 줄을 바꾸지 않고 한 줄로 `flex item`을 배치한다.

![](https://images.velog.io/images/ken1204/post/52f2326a-64a3-4cb9-aa97-bde8a14c64b8/image.png)

### `justify-content: space-around `

`justify-content` 속성은 **주축을 기준으로 flex item을 수평으로 정렬한다.** 다음과 같은 5개의 속성 값으로 다양한 수평 정렬 레이아웃을 만들 수 있다. 자주 사용하는 `space-between`과 비교해보자.

- `space-between` : 첫 번째와 마지막 `flex item`은 주축의 시작 부분과 끝부분에 정렬하고 나머지 `flex item`을 일정한 간격으로 정렬한다.

- `space-around` : 주축을 기준으로 `flex item`을 일정한 간격으로 정렬한다.

![](https://images.velog.io/images/ken1204/post/33988d4f-e9d3-4247-983e-84d192af3540/image.png)

### `align-content`

이 속성은 `flex item`이 여러 줄로 나열되어 있을 때 주축을 기준으로 수직 정렬 방법을 설정하는 속성이다. 일반적으로 자주 사용하는 `align-items` 속성(특히 `center`)은 주축을 기준으로 `flex item`을 수직으로 정렬한다.

> 둘의 차이점은 `align-items` 속성은 **한 줄**로 나열되어 있을 때 사용된다는 것이다! `align-content`는 **여러 줄**일 때다!

![](https://images.velog.io/images/ken1204/post/60837761-155b-4be4-bed7-ec077451a70b/image.png)

> 주의점: `justify-content`, `align-items`, `align-content` 등은 모두 **'주축'**을 기준으로 정렬한다. 따라서 `flex-direction: column` 으로 주축 방향을 전환한다면, 정렬 기준도 전환된다는 점을 유의하자.

### `flex-auto`

`flex-auto` 속성은 유동적으로 크기가 변하는 박스를 만들 때 유용하게 사용된다. `flex: auto` 속성은 `flex: 1 1 auto` 속성과 같다. `flex container`의 크기가 커지면 `flex item`의 크기도 커지고, `flex container`의 크기가 작아지면 `flex item`의 크기도 작아진다. `flex container`의 크기에 영향을 받는 반응형 `flex item`을 만들고 싶다면 `flex: auto` 속성을 사용한다.

만약 아래 이미지처럼 비율이 유지되는 `flex-item`을 적용하고 싶다면 `flex-basis: 33.3%`를 적용하도록 하자.

![](https://images.velog.io/images/ken1204/post/346e28e8-dce5-40d8-a570-e1597243f969/image.png)

### 참고 링크

- [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)

- [Flexbox MDN](https://developer.mozilla.org/ko/docs/Web/CSS/flex)
