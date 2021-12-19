프론트엔드 진영의 프레임워크, 라이브러리 등(React, Angular, Vue)을 공부하다 보면 **단일 페이지 어플리케이션(Single Page Application)** 이라는 용어가 자주 등장한다. 현재 내가 공부하고 있는 React를 이용한 웹 애플리케이션도 SPA에 해당한다.

## SPA가 뭔가요?

> SPA는 기본적으로 단일 페이지로 구성되며, 기존의 서버 사이드 렌더링과 비교할 때, 배포가 간단하며 네이티브 앱과 유사한 사용자 경험을 제공할 수 있다는 장점이 있다. - [출처: PoimaWeb](https://poiemaweb.com/js-spa)

`link` 태그를 사용하는 기존의 화면 전환 방식(Multi Page Application)은 새로운 페이지 요청 시마다 정적 리소스가 다운로드 되고 전체 페이지를 다시 렌더링하는 방식을 사용하므로 새로고침이 발생되어 사용성이 좋지 않다. 그리고 변경이 필요없는 부분까지 포함하여 전체 페이지를 갱신하므로 비효율적이다.

그에 반해 SPA는 모든 정적 리소스를 최초 접근 시 <span style="background-color: lightyellow; font-weight: bold">단 한번만</span> 다운로드한다. 이후 새로운 페이지 요청 시, **페이지 갱신에 필요한 데이터만을 JSON으로 전달받아 페이지를 갱신**하므로 전체적인 트래픽을 감소시킬 수 있고, 전체 페이지를 다시 렌더링하지 않고 변경되는 부분만을 갱신하므로 새로고침이 발생하지 않아 **네이티브 앱과 유사한 사용자 경험을 제공할 수 있다.**

## SPA를 왜 쓰는건가요?

모바일 앱을 사용해본 사용자라면 알 것이다. 부드러운 화면 전환과 함께 사용성, 반응성이 웹사이트보다 훨씬 좋다는 것을 경험했을 것이다.

모바일 사용이 PC를 뛰어넘은 현재, 많은 사용자들은 자연스럽게 모바일 앱에 더 친숙해지고, 모바일 앱의 사용성, 반응성에 익숙해질 것이다. SPA는 이러한 트렌드에 맞추어 대중화되기 시작했다. SPA 또한 <span style="background-color: lightyellow; font-weight: bold">사용자 경험(UX) 경험 향상</span>을 위해 대중화된 모던 웹의 패러다임이다.

단적으로, [다음 웹사이트](https://www.comwel.or.kr/comwel/main.jsp)와 SPA로 만들어진 [이 사이트](https://clelab.io/)를 비교해보자. 확실하게 이해가 될 것이다..! 👍

또 다른 예시로 너무나 직관적인 그림이 있어서 참고하면 좋을 것 같다.
![](https://images.velog.io/images/ken1204/post/f02758c7-7dfa-46b8-a586-62f3af26359f/image.png)

- 출처: [[앵귤러] SPA (Single Page Application)에 대한 고찰](https://paperblock.tistory.com/87)

## SPA의 단점

그러나 SPA가 장점만 있는 것은 아니다. 당장 요즘 핫한 React 진영의 **Next.js**나, Vue.js의 **Nuxt.js** 또한 SPA의 단점을 보완하기 위해 만들어진 서버 사이드 렌더링을 지원하는 라이브러리이다.(엄밀히 말하면 이 CSR+SSR이다) 대표적인 단점 몇 가지를 보도록 하자.

### 1. SEO(검색엔진 최적화) 문제

그러나 SPA로 빌드된 결과물을 보면, `<body></body>`는 텅 비어있고, javascript가 `body`를 바꾸기 때문에 검색엔진은 <span style="background-color: lightyellow; font-weight: bold">이 사이트를 비어있는 사이트로 인식한다.</span> 이는 검색 엔진이 크롤링 할 때 Javascript를 실행하지 않고 긁어가기 때문에 문제가 발생하게 된다.

전통적인 MPA의 경우 사용자 단에서 이미 완성된 형태의 템플릿(HTML에 데이터가 삽입된 상태)을 서버로부터 전달받는다. 이 때문에 검색 로봇이 페이지를 크롤링하기에 적합하다.

> 그런데 요즘 구글의 검색 엔진은 Javascript 처리도 가능하다고 하네요..? 그렇지만 완벽하게 작동하는지 검증이 필요하고, 검색 엔진은 구글 말고 다른 것들도 많긴 합니다. (대표적으로 N사가 있겠네요)

### 2. 초기 구동 속도

SPA는 최초 접속 시 사이트 구성과 관련된 모든 리소스를 한 번에 받아오기 때문에 초기 구동 속도가 상대적으로 느리다. 현대의 웹 애플리케이션은 더구나 하나의 HTML 파일을 제외하고 모두 자바스크립트 파일로 이뤄지기 때문에 파일 자체 또한 무겁다.

### 3. 화면 변하는 모습

SPA는 데이터를 별도로 요청하고 받아와서 화면을 구성하므로 설계에 따라서 화면이 변하는 모습이 사용자에게 노출될 수 있다. 사용자 경험에 좋지 않은 영향일 수 있다.

그러나 2번, 3번 단점은 lazy-loading과 pre-rendering을 통해서 어느 정도 극복이 가능하다.

그럼 먼저 2번, 3번의 단점을 극복할 수 있는 lazy-loading과 pre-rendering을 살펴보도록 하자. 다만, lazy-loading은 이전 [프론트엔드 성능 최적화](https://velog.io/@ken1204/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94-1#%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%97%90%EC%84%9C%EC%9D%98-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94) 글에서 이미 다뤘기 때문에 여기서는 생략하도록 하겠다.

> 간단하게 lazy-loading(지연 로딩)에 대해 설명하면, 최초 접속 시 모든 리소스를 받아오기 때문에 느려지니까, 처음에 모든 리소스를 받아오는 것이 아니라 **특정 URL에 진입할 때 그에 해당하는 조각을 받아올 수 있도록 한다.** (전체가 아니라!)

> <span style="background-color: lightyellow; font-weight: bold">초기 로딩에 걸리는 시간을 라우팅 별로 분산</span>시킨다는 의미로 보면 된다. 물론, 한번 로드된 부분은 더 이상 서버에서 받아오지 않는다. - 출처: [[앵귤러] SPA (Single Page Application)에 대한 고찰](https://paperblock.tistory.com/87)

## 화면 변하는 모습 극복 방안: Pre-rendering

페이지 이동시 데이터 전송이 필요한 경우가 있다. SPA에서는 데이터가 없는 화면이 먼저 표시된 후, 비동기 요청을 통해 데이터를 서버로부터 받아온 후 화면을 재구성하게 된다. 이 경우 로딩 스피너 등으로 화면이 로드되고 있다는 사실을 보여주기도 한다.

![](https://images.velog.io/images/ken1204/post/f5b88718-0f0a-4442-ad25-3aeafe964e11/68747470733a2f2f7468756d62732e6766796361742e636f6d2f506f7461626c65456d6261727261737365644672656e636862756c6c646f672d736d616c6c2e676966.gif)

- 요런거 말이다. React를 사용하는 사람이라면 많이들 알고 있을 코드인 다음 코드를 참고하자. 아래 코드는 컴포넌트가 로드되는 동안 로딩 화면인 `<div>Loading...</div>`을 보여주게 된다.

```html
 <Suspense fallback={<div>Loading...</div>}>
     <Component />
 </Suspense>
```

그런데 요즘은 pre-rendering이라고, **로딩 시에 스켈레톤 화면을(골격이 되는 화면) 표시**해주는 경우도 심심치 않게 보이고 있다. 아래 이미지를 참고하면 된다. (이거 굉장히 적용해보고 싶다. 매우 트렌디해 보인다)
![](https://images.velog.io/images/ken1204/post/a1328588-fdea-4619-8c38-37b7b17367e3/img.gif)

- 출처: [[앵귤러] SPA (Single Page Application)에 대한 고찰](https://paperblock.tistory.com/87)

## 결론

굉장히 두루뭉실하게 알고 있었던 SPA의 장/단점, 사용 이유, 단점을 극복하기 위한 방안 등을 이번 글을 작성하며 조금 더 확실히 하게 되었다! 이렇게 기술에 관련된 글을 읽는 건 확실히 재미가 있다 :)

문제는 pre-rendering을 공부하면서 Next.js까지 손을 대보고 싶어졌다.(다른 것 공부할 시간을 또 빼게 되겠다..) 올해 세워둔 계획까지만 끝내고 얼른 Next.js를 통해 SSR도 같이 경험해보고 싶다.

추가적으로, SEO 단점 극복 방안에 대해 이론적으로 알게 되었지만, 직접 내 프로젝트에 적용해보고 작성해보고 싶어서 일단 보류해뒀다. `react-snap` 라이브러리를 통해 검색 엔진 최적화 방안을 직접 적용해 볼 예정이다!

### 참고 링크

[Single Page Application & Routing](https://poiemaweb.com/js-spa)
[JAM Stack 개념 정리하기](https://pks2974.medium.com/jam-stack-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-17dd5c34edf7)
[SPA(Single Page Application)에 대한 고찰](https://paperblock.tistory.com/87)
[[React] 검색엔진 최적화(SEO):: Prerendering (react-snap)](https://satisfactoryplace.tistory.com/131)
