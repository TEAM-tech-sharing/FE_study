출처: [10분 테코톡 - 나봄의 CORS] https://www.youtube.com/watch?v=-2TgkKYmJt4&list=RDCMUC-mOekGSesms0agFntnQang&index=9

<br/>
프론트엔드와 백엔드 협업 시 다음과 같은 에러 메시지를 본 적이 있을 것이다.

![](https://images.velog.io/images/yena1025/post/d879af86-36ac-4695-b602-905579c551ec/cors%20%EC%97%90%EB%9F%AC%EB%A9%94%EC%8B%9C%EC%A7%80.png)

CORS는 프론트엔드와 백엔드가 협업할 때 종종 만나게 되는 상황이다.
CORS를 이해하기에 앞서, **SOP**라는 개념에 대해 먼저 알아보자.

# SOP (Same Origin Policy)

SOP의 개념은

**다른 출처의 리소스 사용을 제한**하는 **보안** 정책

<br/>
보안 정책이란 말을 보니 <BR/>
다른 출처의 리소스를 사용하면 보안에 위험한데,

SOP라는 정책을 통해 보안에 위험하지 않도록
미리 방지할 수 있다 정도로 의미를 추측할 수 있다.

그런데 '다른 출처'라는 것은 정확하게 무슨 뜻일까?

## 출처(Origin)

다음 그림은 **URL**의 구성을 나타낸 것이다.

![](https://images.velog.io/images/yena1025/post/06af6918-1afd-4ee7-91ec-afb8f8a37092/%EC%B6%9C%EC%B2%98.png)

URL은 앞에서부터 차례대로
Protocol(프로토콜), Host(호스트), Port(포트번호),
Path, Query String(쿼리스트링), Fragment로 이루어져 있다.

URL의 맨 앞 3개의 요소(**프로토콜, 호스트, 포트번호**)를 통해
같은 출처인지 다른 출처인지 판단한다.

**반드시 세 요소가 모두 같아야만 같은 출처**이고,
**하나라도 다르면 다른 출처**라고 판단한다.

> 예외적으로 IE(Internet Explorer)는 Port를 판단 기준에서 제외한다.
> (= 즉, Port가 달라도 같은 출처라고 판단한다)

<BR/>

### Q. 퀴즈 -- 출처 구분하기

다음 중 'http://localhost' 와 동일한 출처인 url은?

1번. https://localhost
2번. http://localhost:80
3번. http://127.0.0.1
4번. http://localhost/api/cors

.
.
.

정답은 2번과 4번이다.

1번은 'http'가 아닌 'https'이므로 Protocol이 다르다.

2번은 http 프로토콜에 80 포트가 붙여져 있는 형태인데
http의 기본 포트가 80 포트이므로 생략 가능하다.

3번은 127.0.0.1의 IP가 localhost임에도 불구하고,
브라우저 입장에서는 URL을 '문자열값'으로 비교하기 때문에 다르다고 판단한다.

4번은 /api/cors가 추가적으로 붙는 location(로케이션)인데,
로케이션은 말 그대로 추가적인 정보일 뿐이어서 /api 앞에까지만 비교한다.

<br/>
<br/>
자, 다시 SOP로 돌아가자.

아까 SOP는 보안 정책이라고 했다.

그러면 '다른 출처의 리소스 사용을 제한' 하는 것이

어째서 보안에 도움이 되는 걸까?

다음 예시를 살펴보자.

### 예시

![](https://images.velog.io/images/yena1025/post/474a0b74-9f87-42a0-ac3f-fe62f6c41db4/%ED%8E%98%EC%9D%B4%EC%8A%A4%EB%B6%81%EC%98%88%EC%8B%9C.png)

사용자가 페이스북을 이용하기 위해 로그인을 한다.

그러면 사용자가 입력한 정보가 페이스북에 들어가고
페이스북 서버는 사용자에게 **인증 토큰을 발급**한다.

이때 해커가 사용자에게
페이스북에 이상한 게시글을 등록하라는
스크립트가 담긴 메일을 보낸다.

사용자가 메일을 클릭하는 순간,
해커가 만든 주소('http://hacker.ck')로 이동하고
사용자가 가지고 있는 페이스북 인증토큰을 이용해
페이스북에게 스크립트의 이상한 요청을 보내게 된다.

SOP 정책이 존재할 경우,
사용자의 인증 토큰을 받은 페이스북은
사용자의 요청이 어디서 온건지 출처(Origin) 확인을 할 수 있다.

이때 해커의 주소(http://hacker.ck)가
자신의 출처(https://www.facebook.com/login,
페이스북 로그인 페이지)와 다르다는 것을 알게 되면
**'Cross-origin'**, 즉 **출처가 다르다고 판단**하여
사용자가 보낸 이상한 스크립트 요청을 받아들이지 않게 된다.

---

여기서는 단순히 이상한 게시글을 올리는 정도의 예시였지만,
만약 송금과 같은 민감한 개인정보들이 개입된 상황이라면
치명적인 보안 문제가 발생할 수 있다.

이렇게 서로 다른 출처의 리소스가 오가는 데서 발생하는 문제를 다루기 위해 등장한 개념이 바로 CORS 이다.
<br/>

# CORS (Cross-Origin Resource Sharing)

사용자가 웹 서비스를 이용하다 보면
다른 출처의 리소스가 필요한 경우가 생긴다.

CORS는 영어를 그대로 번역하면 **'교차 출처 리소스 공유'**이다.
한 마디로 **교차 출처, 즉 서로 다른 출처의
자원을 공유할 수 있게 하는 것**이다.

<BR/>

다음은 Mozilla 사이트의 설명이다.

> CORS는 추가 HTTP 헤더를 사용하여
> 한 출처에서 실행 중인 웹 애플리케이션이
> **다른 출처의 자원에 접근 가능한 권한**을 부여하도록
> **브라우저에게 알려주는 것**이다.

'다른 출처의 자원에 접근 가능한 권한'을
'브라우저에게 알려준다'는 부분이 중요하다.

## CORS 접근제어 시나리오

CORS 접근제어 시나리오는
**CORS 상황에서 브라우저와 서버가 '통신'하는 상황**을 말한다.
다음과 같은 3가지 방식이 있다.

1. 단순 요청
2. Preflight 요청
3. 인증정보 포함 요청

### 1. 단순 요청 (Simple Request)

단순 요청은 preflight 요청 없이 바로 본 요청을 보내고
cross-origin 여부를 확인하는 방식이다.

<img src="https://images.velog.io/images/yena1025/post/dfb5922b-5bc6-49d8-95f1-2f7a68652de5/%EC%8B%AC%ED%94%8C.png" width="600" />

위의 그림에서 Client의 출처(Origin)는 foo.example인 반면,
Server는 모든 출처(\*)의 요청을 허용하고 있다.

즉 CORS가 발생할 수 있는 상황에서도
아무런 장치 없이 본 요청을 보내는 것이 단순 요청 방식이다.

#### 단순 요청 시 만족해야 하는 조건

- GET, POST, HEAD 메서드만 가능
- Content-Type은 다음 중 하나여야

1. application/x-www-form-urlencoded
2. multipart/form-data
3. text/plain

- Header도 다음 중에만 가능

1. Accept
2. Accept-Language
3. Content-Language
4. Content-Type

#### 문제

CORS 개념을 인식 못하는 서버들에게 이러한 단순 요청을 보낼 경우,
CORS 에러가 발생할 수 있다.

<br/>

### 2. 프리플라이트 요청 (Preflight Request)

Preflight에서 pre는 '미리', flight는 '비행'을 뜻한다.
즉 '본격적인 일이 일어나기 전에 뭔가를 미리 한다'는
개념 정도로 이해하면 되겠다.

#### Preflight가 생겨난 배경

CORS라는 개념이 생기기 이전의 옛날 서버들은
브라우저의 SOP가 가능하다는 전제 하에 만들어졌다.

하지만 CORS 개념이 등장하면서 cross-origin 요청이 가능해진 이후,
이러한 옛날 서버들에는 CORS와 같은 보안 장치가 없었다.

그래서 이러한 서버들을 보호하기 위한 새로운 통신 방식으로
등장한 것이 바로 preflight 개념이다.

Preflight 요청은
**본 요청이 가능한지 먼저 서버에게 '사전확인'을 한 뒤
서버가 가능한 경우에만 요청을 하는 것**이다.

교차 출처가 서로 다른 CORS 상황을 인식하지 못하는
서버들을 보호하기 위한 시스템이라 할 수 있다.
<br/>

#### Preflight 요청 절차 상세

1. HTTP 요청 메시지에 있는 Request Method에
   **OPTIONS 메서드**를 넣는다.

   <img src="https://images.velog.io/images/yena1025/post/2dd18009-f19e-41ec-b33b-10a4b89e4fde/preflight.png" width="500" />

2. 그러면 요청이 2번 가게 된다.

   <img src="https://images.velog.io/images/yena1025/post/0ebe4533-eabe-46fe-85ef-1df44104c135/%EC%84%9C%EB%B2%84%EC%9A%94%EC%B2%AD.png" width="500" />

1) Preflight 사전 요청: 실제 요청을 보내도 되는지 서버에게 물어보는 것

   > \*\* 여기서 실패하면 브라우저는 405 Method Not Allowed 에러를 발생시키고, 실제 요청은 서버로 전송하지 않음

2) 실제 요청: Preflight 요청이 성공하면 본 요청을 보내게 됨

<br/>
** 서버에서도 요청 횟수에 맞게 응답을 2번 한다.

1. Preflight에 대한 응답: 본 요청 보내도 된다 vs. 안 된다
2. 실제 요청에서 요구한 데이터를 전달하는 응답

<br/>

#### preflight 요청 내용과 포맷

<img src="https://images.velog.io/images/yena1025/post/d90fa1e4-1951-4147-93e3-c594f81430bb/%EC%82%AC%EC%A0%84%EB%AC%BC%EC%96%B4%EB%B3%BC%EA%B1%B0.png" width="450" />

1. Origin: 이 요청이 어디에서 보내지는지 (브라우저쪽 출처)
2. Access-Control-Request-Method: 실제 요청에 쓰일 메서드를 사용해도 되는지 (GET, POST 등)
3. Access-Control-Request-Header: 실제 요청 시 어떤 추가 헤더를 보낼 수 있는지 (Content-Type 등)

<br/>

#### Preflight 응답 내용과 포맷

<img src="https://images.velog.io/images/yena1025/post/c2c991c6-1230-45a0-a65f-0a25d3affdf2/%EC%9D%91%EB%8B%B5.png" width="500" />

1. Access-Control-Allow-Origin: 이 출처를 허가함
2. Access-Control-Allow-Methods: 이 메서드들을 허가함
3. Access-Control-Allow-Headers: 이 헤더들을 허가함
4. Access-Control-Max-Age: 사전 응답 캐시가 유효한 시간 \*\*

   > **Access-Control-Max-Age**
   > Preflight 방식은 매번 2번의 요청을 보내야 하는데
   > 이는 리소스 사용에 좋지 않다.
   > 따라서 **'캐싱'**을 하여,
   > 첫 번째 요청에서 Preflight 요청이 통과되면
   > 그 다음 Preflight 요청은 건너뛰고 바로 본 요청을 보낼 수 있도록 한다.
   > (즉, Access-Control-Max-Age = 캐싱이 유지되는 시간)

   <br/>

### 3. 인증정보 포함 요청 (Credential Request)

**인증 관련 헤더**를 포함할 때 사용하는 요청 방식이다.

JWT 토큰을 클라이언트에서 자동으로 담아서 보내고 싶을 때
**요청 메시지의 Access-Control-Allow-Credentials 항목을 include로 지정**한다.

중요한 점은 **서버 측에서도 마찬가지로 이 설정을 해줘야** 한다는 것이다.

> (서버 응답 메시지에서 다음 내용 작성)

1. Access-Control-Allow-Credentials: true
2. Access-Control-Allow-Origin: "요청을 받을 출처"
   (와일드카드 \* 을 사용해서 모든 출처를 허용해버리면 에러가 발생할 수 있다. 따라서 Access-Control-Allow-Credentials을 허용할 구체적인 출처를 명시해주어야 한다.)

<br/>

## CORS 해결하기

CORS 에러를 해결하는 데는 다음과 같은 3가지 방법이 있다.

### 1. 프론트엔드에서 Proxy 서버 설정 (개발환경)

웹팩의 webpack-dev-server 라이브러리가 제공하는
proxy 기능을 사용하여 CORS 정책을 우회한다.

웹팩 설정 파일(예: vue.config.js)에 다음 내용을 추가하면 된다.

```
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'https://api.evan.com',
        changeOrigin: true,
        pathRewrite: { '/api': '/' },
      },
    }
  }
}
```

위의 코드를 설명하면,

로컬 환경에서 **`/api`**로 시작하는 URL로 보내는 요청에 대해
브라우저는 같은 출처인 **`localhost:8000/api`** 서버로 요청을 보낸 것으로 인식하지만,

뒤에서는 웹팩이 실제 서버인 **`https://api.evan.com`**으로
요청을 보내주기 때문에

서로 다른 출처에서 자원을 주고받음에도 불구하고
마치 SOP 정책을 지킨 것처럼 브라우저를 속일 수 있다.

즉, 프록싱을 통해 CORS 상황을 우회할 수 있는 것이다.
<br/>

**다만 이 방법은 실제 프로덕션 환경에서
클라이언트 서버 출처와 API 서버의 출처가 같은 경우에 사용**하는 것이 좋다.

로컬 개발 환경에서는 웹팩이
요청을 프록싱(우회)해주니 아무 이상이 없지만,

일단 어플리케이션을 빌드하고 서버에 올리고 나면
더 이상`webpack-dev-server`가 구동하는 환경이 아니기 때문에
이상한 곳으로 API 요청을 보낼 수 있다.

예를 들어 API 서버의 출처는 `https://api.evan.com`이고
클라이언트쪽 서버의 출처는 `https://www.evan.com`이라면
다음과 같은 상황이 발생할 수 있다.

```
fetch('/api/me');
로컬환경에서는...
GET https://api.evan.com/me (200 OK)

그러나 실제 서버에는 프록싱 로직이 없음...
GET https://www.evan.com/api/me (404 Not Found)
```

<br />

### 2. 서버에서 직접 헤더에 설정

서버에서 Access-Control-Allow-Origin 항목에
요청받을 출처를 정확하게 명시해주는 방법이다.
근데 매번 이렇게 해주는 건 귀찮아서
주로 스프링 부트의 기능을 이용한다고 한다.

<br />

### 3. 스프링 부트 이용

백엔드 컨트롤러 단에서 **@CrossOrigin** 키워드를 통해
origin을 구체적으로 명시한다.
(\*@CrossOrigin 다음 소괄호 안에 origin을 작성할 때,
와일드카드를 쓰고 credentials를 true로 주면 오류가 발생하니
반드시 구체적인 origin을 명시한다)

만약 여러 개의 api에 전부 이런 처리를 하고 싶다면
전역으로 설정해준다.
(라이브 코딩에서는 configCors 같은 폴더에
@CrossOrigin 함수 부분만 모듈화해두었음)

<br />

#### 결론

현업에서는 CORS 상황이 되도록 발생하지 않도록 로직을 짠다고 한다.
그리고 발생한다 해도 보통 백엔드 쪽에서 해결하는 경우가 많다고 들었다. 프론트엔드에서는 SOP, CORS 개념과
해결방안 1번 정도만 기억해두면 될 것 같다.
