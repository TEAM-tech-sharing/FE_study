## CSS in JS

### 탄생 배경
css의 여러 문제점을 효율적으로 제어하기 위해 **CSS-in-CSS**, **CSS-in-JS**와 같은 방식이 등장.

#### CSS-in-CSS
CSS-in-CSS에는 대표적으로 **CSS module**과 **CSS 전처리기**가 있습니다.

- **CSS module**은 클래스를 자동으로 로컬 스코프로 지정해 클래스가 중첩되는 것을 방지할 수 있는 장점이 있습니다. css의 규모가 커질 때 효율적으로 유지 보수가 용이합니다.
    ```css
    /* box.module.css */
    /* class는 Box_box_fjqw.. */
    .box{
        background: black;
    }
    ```
- **CSS 전처리기**는 css가 프로그래밍 문법적으로 부족한 점들을 보완합니다. 예를 들어 변수 선언, mixin, 반복문, 조건문 등을 이용해 스타일을 제어할 수 있습니다.
    ```scss
    $font-size-12: 12px;
    $line-height-12: 16px;
    $letter-spacing-12: -0.005em;

    @mixin text-style($size, $color: false) {
        @if ($size == 12) {
              font-size: $font-size-12;
              line-height: $line-height-12;
              letter-spacing: $letter-spacing-12;
        }
        ...
    }
    ```

## CSS-in-JS의 특징
CSS-in-JS는 이름 그대로 스타일 정의를 javascript로 작성된 컴포넌트에 바로 삽입하는 방법입니다. 

```javascript
// styled-components, emotion의 경우 템플릿 리터럴과 CSS 기능을 이용
const BoxWraper = styled.div`
    width: 10px;
    height: 10px;
    backgorund: black;
`

const Box = () => {
    return (
        <BoxWraper>
            ...
        </BoxWraper>
    )
}
```

### runtime 개념 
**runtime** 개념을 도입해 동적인 스타일링이 가능합니다. 즉, build 할 때 모든 스타일이 생성하는 게 아닌 **필요할 때만 특정 스타일을 지정**할 수 있습니다.
```javascript
const Box = () => {
    return (
        <BoxWraper color="red">
            ...
        </BoxWraper>
    )
}

const BoxWraper = styled.div`
    backgorund: ${props.color === 'red' ? 'red' : 'black'};
`
```


#### 컴포넌트의 특징
css를 사용할 때처럼 가장 상위의 파일에 css 파일을 묶을 필요가 없습니다. 이로 인해 **분리의 편리함**과 **스코프가 있는 선택자**를 만들 수 있습니다. 

> css module과 마찬가지로 클래스 이름을 자동으로 중복되지 않게 생성해 줍니다.
> ```html
> <div class="sc-jrsJWt...">
> ```

#### 미사용 코드 검출 (죽은 코드 제거)
사용하지 않는 js 코드를 사전에 발견할 수 있듯, 컴포넌트로 구현한 스타일들을 사용하지 않는다면 **사전에 발견**할 수 있습니다.

### CSS-in-JS의 단점
- 러닝 커브
- 새로운 의존성 발생
- 별도의 라이브러리 설치로 인해 CSS-in-CSS에 비해 느린 속도

### 어느 경우에 사용?
- **컴포넌트 기반 웹 사이트**인 경우 ( SPA와 같은 ) **필요한 컴포넌트 페이지의 CSS 스타일 요소**만 로딩할 수 있어서 효율적입니다. ( CSS-in-CSS의 경우 모든 css 스타일 요소까지 로딩 )

- **인터렉티브한 웹 사이트**를 구현해야 할 경우 prop이 변할 때마다 동적으로 css를 로딩해야 하기 때문에 **모든 요소를 미리 로딩한 CSS-in-CSS**가 효율적일 수 있습니다.
