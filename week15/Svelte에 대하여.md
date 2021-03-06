Svelte는 인터랙티브 웹 사이트를 구축하는 툴이다.

2021년 stackoverflow 개발자 설문조사
'가장 많이 사랑받는 웹 프레임워크'에서 1위를 차지했다.

## 오늘 알아볼 것

1. Svelte가 무엇이고 어떻게 동작하는지
2. 리액트와 무엇이 다른지
3. 어떻게 리액트보다 더 빠른지
4. 왜 개발자들에게 이토록 인기가 많은지
5. 누가 Svelte를 배워야 하는지
6. 어떻게 Svelte 코드가 동작하는지 (feat. 예시)

## Svelte가 무엇이고 어떻게 동작하는가, 리액트와는 무엇이 다른가

Svelte는 리액트처럼 인터랙티브 웹을 구축하지만,
구축하는 방법은 리액트와 다르다.

리액트, 뷰와 다르게 Svelte는 라이브러리나 프레임워크가 아니라
**컴파일러**에 가깝다.

리액트로 웹 서비스를 빌드, 제작 후 배포하면
유저가 웹 사이트에 들어올 때 2가지 종류의 코드를 다운받아야 한다.

1. 개발자가 작성한 코드

- 애플리케이션을 구성하는 코드

2. 리액트의 코드

- 개발자가 작성한 코드가 리액트를 사용하므로
  애플리케이션을 돌리려면 유저가 리액트도 다운받아야 하는 것

이는 일종의 표준화된 방식으로서,
예를 들어 개발자가 jQuery를 사용해서 개발한 애플리케이션을 사용하려면
유저도 jQuery를 다운받아야 그 애플리케이션을 실행할 수 있다.

Svelte는 '스르륵 사라지는 마법의 UI 프레임워크'라고 공홈에 정의되어 있다.
Svelte로 코딩 후 배포 시 Svelte가 알아서 코드를 분석하고
브라우저가 바로 이해 가능한 JS 코드로 자동 컴파일한다.
즉, 브라우저에게 별도로 Svelte가 무엇인지 설명할 필요가 없으며
유저가 애플리케이션 구성 코드 외에 Svelte 코드를 또 다운받을 필요가 없는 것이다.

이러한 이유로 Svelte 코드가 쓰인 앱의 크기는
리액트나 뷰로 작성된 서비스에 비해 훨씬 작다.
예를 들어, Svelte로 todoApp 하나를 작성했을 때 크기가 3KB라면
같은 todoApp을 리액트로 작성하면 크기가 45KB가 된다.

또한 Svelte는 리액트나 뷰보다 훨씬 빠르다.
프레임워크를 쓰지 않는 Vanilla JS와 거의 같은 스피드를 보인다.
![](https://velog.velcdn.com/images/yena1025/post/636368ca-e6e1-401a-8cb2-e96139cdc317/image.png)

또 Svelte는 이러한 자동 컴파일 기능 덕분에
Virtual DOM을 쓰지 않아도 된다.

Virtual DOM은 앱의 UI를 나타내며, 리액트의 경우 메모리에 보관하고 있다.
이는 유저가 보고 있는 실제 UI와 때때로 동기화되는데,
새로고침이 필요한 요소를 찾아 유저에게 변경된 UI를 보여주는 것이다.

리액트가 처음 출시되었을 때
변경사항 체크를 위해 UI 사본을 메모리에 저장하는 일은
그 전에는 없었다.

하지만 Svelte에서는 이러한 리액트에서 더 나아가
Virtual DOM 자체가 필요가 없는데, 이는
컴파일러가 더욱 최적화된 JS 코드를 만들어서
Virtual DOM보다 더 빠른 속도로 UI를 새로고침하기 때문이다.

### Svelte가 왜 이렇게 인기가 많은가?

가장 큰 이유는 '개발 경험' 때문이다.
배우기 쉽고, 직관적이다.

#### Svelte 컴포넌트 구성 요소

![](https://velog.velcdn.com/images/yena1025/post/47057486-f3ee-4651-bf33-dd074f38005e/image.png)

1. JS script 태그
2. CSS style 태그
3. HTML 코드

<BR/>

출처: [노마드코더 스벨트 10분 소개 영상](https://www.youtube.com/watch?v=Y7PHBSqDfvE)
