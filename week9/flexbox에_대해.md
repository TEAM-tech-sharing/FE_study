출처: [네이버 D2 - flexbox로 만들 수 있는 10가지 레이아웃] [https://d2.naver.com/helloworld/8540176](https://d2.naver.com/helloworld/8540176)

# 레이아웃 1 - 스크롤 없는 100% 레이아웃

**전체 페이지를 구성할 때** 자주 사용할 수 있는 레이아웃이다.

**콘텐츠의 길이에 상관없이 브라우저 화면 전체를 채운다.**

#### 구현예제: [1. 스크롤이 없는 100% 레이아웃](https://codepen.io/witblog/pen/MqGNeM)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_04.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_04.png)

여기서 **웹 페이지의 위에 있는 메뉴 영역의 높이는 고정**되어 있다.

부모 영역에서 **메뉴 영역을 뺀 나머지 영역 전체를 자식 요소가 채운다.**

** 콘텐츠의 길이가 길어지면 **스크롤 막대가 브라우저가 아니라 **콘텐츠 영역**에 나타난다.

### 기본 CSS

.flex item에 `flex: 1` 속성을 적용해 flex item이 빈 공간을 채우게 한다.

```
.flex_container {  // 부모 요소
	display: flex;
	flex-direction: column;
	height: 100%;
}

.flex_item {  // 자식 요소
	flex: 1;  /* flex: 1 1 0 */
	overflow: auto;
}
```

## `flex-grow`

flex-grow나 flex-shrink에서 **0은 false, 1 이상은 true**를 의미한다고 보면 된다.

flex-grow는 **flex item(자식 요소)의 확장**과 관련된 속성이며, `0`과 양의 정수를 속성값에 사용한다.

**속성값이 `0`일 경우**
flex container의 크기가 커져도 **flex item의 크기가 커지지 않고 원래 크기로 유지**된다.

**속성값이 `1` 이상일 경우**
flex item의 원래 크기에 상관없이 **flex container를 모두 채우도록 flex item이 커진다.**

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_07.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_07.png)

## `flex-shrink`

flex-shrink는 **flex item의 축소**와 관련된 속성이다.

`0`과 `1`를 속성값에 사용하며, 기본값은 `1`이다.

**속성값이 `0`일 경우 **
flex container의 크기가 flex item의 크기보다 작아져도 **flex item의 크기가 줄어들지 않고 원래 크기로 유지**된다.

**속성값이 `1`일 경우 **
flex container의 크기가 flex item의 크기보다 작아지면
**flex item의 크기가 flex container의 크기에 맞추어 줄어든다.**

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_08.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_08.png)

## `flex-basis`

flex-basis는 **flex item의 기본 크기를 결정**하는 속성이며, 기본값은 `auto`이다.

`width` 속성에서 사용하는 **모든 단위(px, %, em, rem 등)를 속성값에 사용**할 수 있다.

또한 flex-basis의 값을 `30px`이나 `30%`와 같이 설정하면 flex item의 크기가 고정된다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_09.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_09.png)

flex-basis 속성에서 `auto`와 함께 자주 사용하는 속성값이 `0`이다.

flex-basis 값을 **`0`으로 설정**하면 콘텐츠의 크기와 상관없이

**flex container를 기준으로 크기가 결정**된다. (1:1:1: ...)

**`auto`로 설정**하면 **콘텐츠의 크기에 맞춘다.**

> 주의: flex-basis를 **0으로 선언**할 때에는 **단위와 함께 설정**해야 한다.
> 예: flex-basis: 0px, flex-basis: 0%

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_10.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_10.png)

## `flex : 1`

`flex-grow` 속성, `flex-shrink` 속성, `flex-basis` 속성을 축약한 속성이다.

`flex` 속성의 값으로 **정수 하나만 선언**하면 **`flex-grow` 속성의 값**이 된다.

나머지 shrink와 basis는 각각 1, 0으로 고정되어 들어간다.

예: `flex: 2`는 `flex: 2 1 0`을 가리키고, `flex: 3`은 `flex: 3 1 0` 등

<img src="https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_06.png" width="400" />

**flex-grow가 1**이고, **flex-shrink도 1**이다.
즉, ** flex container의 크기에 따라 flex item의 크기도 커지거나 작아진다**는 의미이다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_11.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_11.png)

# 레이아웃 2 - 내비게이션

웹 페이지에서 자주 사용하는 레이아웃이다.

메뉴의 대부분을 내비게이션 영역의 왼쪽으로 정렬하고, 1~2개의 메뉴(주로 GNB(global navigation bar)나 로그인 버튼)만 오른쪽으로 정렬하는 배치이다.

#### 구현 예제: [2. 네비게이션](https://codepen.io/witblog/pen/WgJVGY)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_12.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_12.png)

### 기본 CSS

```
.flex-container {
	display: flex;
}

.flex-item {
	flex: none;  /* flex: 0 0 auto */
}

.flex-item-gnb {
	margin-left: auto;
}
```

## `flex: none`

내비게이션 구조에서는 보통 'logo, 'search', 'gnb' 요소가 커지거나 작아지지 않게 디자인한다.

**flex item(자식 요소)의 크기를 고정**하려면 반드시 **`flex: none` 속성을 적용**해야 한다.

> **`flex` 속성의 기본값**은 **`flex: 0 1 auto`**
> flex container의 크기가 커져도 flex item의 크기는 그대로,
> flex item 전체의 크기보다 작아질 때는 같이 작아지고,
> flex container의 크기 변동이 없을 때는 콘텐츠 크기에 맞춤

`none` 외에도 `initial`, `none`, `auto` 키워드로 `flex` 속성을 축약하여 표현할 수 있다.

![](https://images.velog.io/images/yena1025/post/78be5ac7-8d01-436c-b19a-0409f2f9a294/image.png)

다음은`flex` 속성의 값에 따른 flex item의 크기 변화를 그림으로 나타낸 이미지이다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_13.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_13.png)

## `margin-left: auto`

요소의 바깥 여백(= margin)에 `auto`를 적용하면 flex item의 상하좌우 배치가 가능하다.

각 상하좌우 방향에 따라 다음과 같이 auto 속성값을 사용할 수 있다.

- `margin-right: auto`: 바깥 여백이 오른쪽의 모든 공간을 차지하기 위해 flex item을 오른쪽에서 왼쪽으로 민다.
- `margin: 0 auto`: 바깥 여백이 flex item을 양쪽에서 밀기 때문에 flex item이 수평 중앙에 위치한다.
- `margin-left: auto`: 바깥 여백이 왼쪽의 모든 공간을 차지하기 위해 flex item을 왼쪽에서 오른쪽으로 민다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_14.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_14.png)

- `margin-bottom: auto`: 바깥 여백이 아래쪽의 모든 공간을 차지하기 위해 flex item을 아래쪽에서 위쪽으로 민다.
- `margin: auto 0`: 바깥 여백이 flex item을 위아래로 밀기 때문에 flex item이 수직 중앙에 위치한다.
- `margin-top: auto`: 바깥 여백이 위쪽의 모든 공간을 차지하기 위해 flex item을 위쪽에서 아래쪽으로 민다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_15.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_15.png)

# 레이아웃 3 - 브라우저 화면 아래에 붙는 Footer

바로 위의 내용과 연결된다.

보통 콘텐츠의 길이가 화면보다 짧으면 footer는 콘텐츠가 짧아진 만큼 위로 올라간 위치에 표시되는데, lexbox를 사용하면 콘**텐츠의 길이와 상관없이 항상 화면 아래에 표시되는 푸터**를 만들 수 있다.

#### 구현 예제: [3. 바닥에 붙는 푸터](https://codepen.io/witblog/pen/yxjmaW)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_16.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_16.png)

### 기본 CSS

```
.flex-container {
	display: flex;
	flex-direction: column;
	height: 100%;
}

.flex_item {
	margin-top: auto;
}
```

# 레이아웃 4 - 정렬이 다른 메뉴

첫 번째와 마지막 자식 요소는 부모 요소의 양쪽 끝에 붙어있고, 두 번째 자식 요소는 부모 요소의 중앙에 위치하는 레이아웃이다.

#### 구현예제: [4. 정렬이 다른 메뉴](https://codepen.io/witblog/pen/dqKbMK)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_18.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_18.png)

### 기본 CSS

flex container에 `justify-content: space-between`을 적용하여 구현이 가능하다.

```
.flex-container {
	display: flex;
	justify-content: space-between;
}

```

### `justify-content`

`justify-content` 속성은 주축을 기준으로 flex item을 수평으로 정렬한다.

- `flex-start`(기본값): 주축의 시작 부분을 기준으로 flex item을 정렬한다.
- `center`: 주축의 중앙을 기준으로 flex item을 정렬한다.
- `flex-end`: 주축의 끝부분을 기준으로 flex item을 정렬한다.
- `space-around`: 주축을 기준으로 flex item을 일정한 간격으로 정렬한다.
- `space-between`: 첫 번째와 마지막 flex item은 주축의 시작 부분과 끝부분에 정렬하고 나머지 flex item을 일정한 간격으로 정렬한다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_19.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_19.png)

## 🍞 space-around와 space-evenly의 차이?

### **space-around**

아이템들의 **“둘레(around)”에 균일한 간격**을 만들어 준다.

### **space-evenly**

아이템들의 **사이와 양 끝에 균일한 간격**을 만들어 준다.

<img src="https://images.velog.io/images/yena1025/post/0dc180ba-6bc1-4bf1-a7a9-6435364e2756/image.png" width="500" />

# 레이아웃 5 - 폼 레이블 수직 중앙 정렬

폼 요소의 레이블을 수직 중앙에 정렬하는 레이아웃이다.

#### 구현예제: [5. 폼 타이틀 수직 중앙정렬](https://codepen.io/witblog/pen/aaKoBV)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_20.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_20.png)

### 기본 CSS

```
.flex_container {
	display: flex;
	align-items: center;
}
```

### `align-items: center`

`align-items` 속성은 주축을 기준으로 flex item을 수직으로 정렬한다.

- **`stretch`(기본값): flex item의 높이를 늘려 flex container의 전체 높이를 채운다.**
- `flex-start`: 교차축의 시작 부분을 기준으로 flex item을 정렬한다.
- `center`: 교차축의 중앙을 기준으로 flex item을 정렬한다.
- **`baseline`: 글꼴의 기준선인 baseline을 기준으로 flex item을 정렬한다.**
- `flex-end`: 교차축의 끝부분을 기준으로 flex item을 정렬한다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_21.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_21.png)

<img src="https://images.velog.io/images/yena1025/post/6e1653b8-ea14-48fb-b6ff-9a080f95daf8/image.png" width="320" />

# 레이아웃 6 - 중앙 정렬 아이콘

자식 요소인 아이콘이 **부모 요소의 정중앙에 위치**하는 레이아웃이다.

#### 구현예제: [6. 중앙정렬 아이콘](https://codepen.io/witblog/pen/wEXwev)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_22.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_22.png)

중앙 정렬 아이콘은 두 가지 방법으로 구현할 수 있다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_23.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_23.png)

1. **flex container에 `align-items: center` 와 `justify-content: center`를 적용한다.**

```
.flex_container {
	display: flex;
	align-items: center;
	justify-content: center;
}
```

2. **flex item에 `margin: auto`를  적용한다.**

```
.flex_container {
	display: flex;
	height: 100%;
}

.flex_item {
	margin: auto;
}
```

# 레이아웃 7 - 유동 너비 박스

flex container의 크기에 따라 flex item의 크기가 콘텐츠의 크기보다 줄어드는 레이아웃이다.

#### 구현예제: [7. 유동 너비 박스](https://codepen.io/witblog/pen/oPyvEa)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_24.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_24.png)

유동 너비 박스에서 부모 요소의 크기와 자식 요소의 크기가 변하는 모습이다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_25.gif](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_25.gif)

### 기본 CSS

```
.flex_container {
	display: flex;
}

.flex_item {
    /* flex : initial; */
	max-width: 300px;
}
```

**flex item에 `max-width: 300px`를 선언하면
flex container의 크기가 300px 이상으로 커져도
flex item은 300px까지만 커진다.**

flex item에 따로 `flex` 속성을 명시하지 않으면 flex item에는 **`flex` 속성의 기본값(initial)**이 적용된다. (**`0 1 auto`**)

**flex container의 크기가 커질 때는 flex item의 크기가 변하지 않지만**

**flex container의 크기가 작아지면 flex item의 크기가 작아진다.**

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_26.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_26.png)

# 레이아웃 8 - 말줄임과 아이콘

flex container의 크기가 작아 **flex item의 텍스트를 모두 표시할 수 없을 때**

**줄임표(`…`)가 나타나게 하는 레이아웃**이다.

이때 **텍스트 영역 옆에 있는 아이콘의 크기는 고정**되어 있다.

#### 구현예제: [8. 말줄임 + 아이콘](https://codepen.io/witblog/pen/eLKOrG)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_27.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_27.png)

### 기본 CSS

```
.flex_container {
	display: inline-flex;
	max-width: 100%;
}

.text {
  	/* flex: 0 1 auto; (initial) */
	overflow: hidden;
	text-overflow: ellipsis;
	white-space: nowrap;
}
```

### `display: inline-flex` 

flex container를 **인라인 + 블록** 요소로 만드는 속성이다.

`display: flex` 속성을 적용한 flex container는 기본적으로 블록 요소이다.

블록 요소는 브라우저 화면의 한 줄 전체를 차지하지만, 인라인 블록 요소는 한 줄의 일부 영역만 차지한다.

<img src="https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_28.png" width="400" />

flex container에 적용한 **`max-width: 100%`** 속성은 **flex item인 텍스트 영역(`.text` 클래스)의 길이가 유동적으로 변할 수 있게 한다.**

그래서 텍스트가 **아이콘을 제외한 나머지 공간을 가득 채운다.**

텍스트 영역이 줄어들 때 나타나는 **줄임표는 `text-overflow: ellipsis`**으로 구현한다.

# 레이아웃 9 - 위아래로 흐르는 목록

flex item을 먼저 위아래로 정렬하고,
flex item가 flex container를 벗어나면 줄을 바꿔 다시 위아래로 정렬한다.

#### 구현예제: [9. 상하 정렬 롤링 리스트](https://codepen.io/witblog/pen/vzrBVw)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_29.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_29.png)

### 기본 CSS

**flex container**에 필요한 속성을 적용한다.

```
.flex_container {
	display: flex;
	flex-direction: column;
	flex-wrap: wrap;
	justify-content: space-around;
	align-content: space-around;
}
```

### `flex-direction: column`

`flex-direction`의 기본값은 수평으로 흐르는 `row`이다.

콘텐츠를 수직으로 흐르게 하려면 `flex-direction`의 값을 `column`으로 설정한다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_30.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_30.png)

### `flex-wrap: wrap` 

**flex item이 flex container를 벗어났을 때 줄을 바꾸는 속성**이다.

`white-space` 속성과 동일하게 작동한다.

속성의 기본값인 `nowrap`은 **flex item의 전체 크기가 flex container보다 커져도 줄을 바꾸지 않고 한 줄로 flex item을 배치한다**. (item이 container 밖으로 튀어나가기도 함)

`flex-wrap: wrap` 속성은 **flex item의 전체 크기가 flex container보다 크면 줄을 바꿔서 그 다음 줄로 flex item을 배치**한다. (item이 container 밖으로 튀어나가지 않는다)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_31.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_31.png)

\*\* **`flex-flow` **속성을 통해 **`flex-direction` **속성과** `flex-wrap` **속성을 **단축**하여 쓸 수도 있다.

```
.flex_container {
	display: flex;
	flex-flow: column wrap;
  ...
}
```

### `align-content` 

`align-content` 속성은 flex item이 **여러 줄로 나열되어 있을 때** 주축을 기준으로 수직 정렬 방법을 설정하는 속성이다.

속성값은 기본적으로 align-items와 동일하며, baseline 대신에 space-around와 space-between이 추가되어 있다.

- **`stretch`(기본값): flex item의 높이를 늘려 flex container의 전체 높이를 채운다.**
- `flex-start`: 교차축의 시작 부분을 기준으로 정렬한다.
- `center`: 교차축의 중앙을 기준으로 정렬한다.
- `flex-end`: 교차축의 끝부분을 기준으로 정렬한다.
- `space-around`: 교차축을 기준으로 flex-item을 일정한 간격으로 정렬한다.
- `space-between`: 첫 번째와 마지막 flex item은 교차축의 시작 부분과 끝부분에 정렬하고 나머지 flex item을 일정한 간격으로 정렬한다.

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_32.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_32.png)

### align-items와 비교

> align-items는 flex item이 여러 줄이 아닌 **한 줄로 나열되어 있을 때**
> 수직 정렬 방법을 설정하는 속성이다.

# 레이아웃 10 - 가로세로 비율을 유지하는 반응형 박스

브라우저의 크기에 따라 **박스의 가로와 세로 크기가 동일한 비율로 늘어나거나 줄어드는 반응형 레이아웃**이다.

#### 구현예제: [10. 가로세로 비율을 유지하는 반응형 박스](https://codepen.io/witblog/pen/LJrPoq)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_34.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_34.png)

### 기본 CSS

```
.flex_container {
	display: flex;
	flex-wrap: wrap;
}

.flex_item_list {
	flex-basis: 33.3%; /* 상위 너비에서 요소의 갯수만큼 나눈 퍼센트 */
	display: flex;
	flex-direction: column;
}

.flex_item_image {
	flex: auto;  /* 1 1 auto; */
}
```

### `flex-basis 고정`

목록을 이루는 항목 요소가 **일정한 비율로 유지**되도록 
`flex-basis: 33.33%` 속성을 적용한다.
(**flex-basis에 단위를 주면 특정 값으로 고정** 가능)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_35.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_35.png)

### `flex: auto`

**이미지 박스**는 유동적으로 반응할 수 있도록 `flex: auto` 속성을 적용한다.
(`1 1 auto`와 동일)

![https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_36.png](https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_36.png)

<br/>

---

여기까지 flexbox의 총 10가지 레이아웃과 함께 관련된 여러 가지 속성들을 알아보았다.

평소에 헷갈렸던 flex 속성들을 완전히 정리해볼 수 있어서 좋았고,

footer를 margin을 통해 배치하는 것과 같은 꿀팁도 얻을 수 있었다.

### 추가 자료

flexbox의 활용에 관해 더 자세한 내용이 궁금하다면 다음 자료를 참고해보자.
(원본 사이트에 있던 링크들 중 들어가보고 사이트가 없어진 것은 제외하였다)

- "[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)"
- "[Typical use cases of Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Typical_Use_Cases_of_Flexbox)"
- "[플렉스 박스 레이아웃](https://poiemaweb.com/css3-flexbox)"
