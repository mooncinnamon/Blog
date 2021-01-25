# HTTP 기초

## Tags

- TAG
    - WEB
    - Network
    - Study

## Category

Study

## Content

### 들어가며

Web 을 공부할때 Framework 위주로 공부하다보면 넘어가는게 HTTP다. 사실 학부생 시절에 네트워크 수업을 들을때나 자세히 HTTP를 보지 짬 내서 HTTP를 공부하는 일은 잘 없다. 그래서 이번 기회에 HTTP 공부를 하면서 정리를 해보자 한다.

### 0. 뭘 공부하지?

- 리소스가 어디서 오는가
- 웹 트랜잭션이 어떻게 동작하는가
- HTTP통신에 사용하는 메시지의 형식
- HTTP와 TCP
- 여러 종류의 HTTP 프로토콜
- 여러 Web Application

### 1. 웹 클라이언트와 서버

HTTP 프로토콜로 통신하는 클라이언트와 서버를 HTTP 클라이언트와 HTTP 서버라고 부른다.

Web Client는 HTTP 요청을 보내는 클라이언트며, 우리가 가장 쉽게 보는 웹 클라이언트는 브라우저다.

Web Server는 HTTP 응답을 보내는 서버이고 클라이언트가 요청한 데이터를 넘겨주는 역할을 한다.

### 2. 리소스

웹에서 제공하는 모든 콘텐츠는 웹 리소스에 포함된다.

웹 리소스를 구분하기위해 Header에 미디어 타입을 포함해서 던지는데 Header의 Content-Type이 바로 그것이다.

![HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__5.59.11.png](HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__5.59.11.png)

[ CSS 요청에 대한 Response Headers의 Content-Type 에 text/css 가 들어있는 모습 ]

이는 MIME Type인데 (Multipurpose Internet Mail Extensions) 이름 그대로 메일 시스템에서 메시지가 오갈때 생긴 문제점을 해결하기 위해 만들어졌다.

MIME TYPE은 주 타입 / 속성 타입 ( primary object type/specific subtype) 으로 되어있다.

수백가지의 MIME TYPE이 존재하고, 베타테스트중인 MIME들도 다수 존재한다.

웹 서버 리소스의 위치를 찾기위해 클라이언트는 URI를 사용하여 웹 서버에 필요한 리소스를 요청한다. URI는 Uniform Resource Identifier 로 URL과 URN이 이에 해당하는데 일반적으로 URL을 사용하기 때문에 URI를 물어보면 보통 URL을 질문한다.

```bash
http://localhost:3000/index.html
```

예제 코드의 웹 서버에서 index.html을 요청하는 URL이다.  자세히 알아보면

1. http:// 은 sheme이라고 불리는데 Http 프로토콜을 사용하라는 뜻이다. 보통 웹 클라이언트에서는 http를 사용한다.
2. [localhost:3000](http://localhost:3000) 은 인터넷 주소를 이동하고 해당 주소로 이동하라는 뜻이다.
3. index.html 은 리소스를 가져오라는 뜻이다.

이 외 URN도 있다. 하지만 리소스 위치를 분석하기 위한 인프라 지원이 미흡하여 아직까지 실험중인 상황이다.

### 3. 트랜잭션

보통 DB에서 트랜잭션이란 단어를 많이 사용하지만 클라이언트가 웹 서버와 리소스를 주고받기 위해 HTTP 트랜잭션 요청을 보낸다고 말한다. 

![HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__6.46.08.png](HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__6.46.08.png)

[ 예제에서 트랜잭션이 이루어진 모습 ]

Request URL에 Method를 웹 클라이언트에서 요청하면 Response Header에 Content-Type 외에 여러 정보를 포함해서 웹 서버가 보내준다.

HTTP에서는 보통 5개의 Method를 사용하는데 한 개의 요청 당 한 개의 Method를 가진다. 

- GET - 서버에서 클라이언트에 지정한 리소스를 보내라
- PUT - 클라이언트에서 서버로 보낸 데이터를 저장하라 (보통 수정시 사용하는 건 overwrite가 발생하기 때문)
- POST - 클라이언트 데이터를 서버에 보내라
- DELETE - 지정된 리소스를 서버에서 삭제하라
- HEAD - 지정한 리소스에 대한 응답에서 HTTP 헤더 부분만 보내라

![HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__6.58.25.png](HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__6.58.25.png)

[ curl 명령으로 HEAD Method를 요청한 결과 ]

또한, HTTP는 응답 시 상태코드를 같이 던져주는데 이는 클라이언트가 요청의 상태 확인을 할 용도로 사용된다.

자주 사용되는 상태코드는 아래와 같다

- 200 - 성공적으로 응답하였다.
- 302 - Redirect를 하라
- 404 - Resource가 없다.
- 500 - 웹 서버에 장애가 있다.

상태 코드는 일반적으로 코드와 함께 reason phrase 도 같이 보낸다. 이는 응답에 대한 설명을 담고있지만, 단지 설명만을 하기 위한 메시지라 응답 처리에는 코드가 필요하다.

3xx는 리다이렉트를 의미하고 4xx와 5xx는 둘 다 장애 코드를 의미하지만 차이가 있다.

![HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.12.39.png](HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.12.39.png)

[ 출처 - 트위터 [https://twitter.com/nsy6228/status/1338461932228026374](https://twitter.com/nsy6228/status/1338461932228026374)]

상태코드에 대한 자료는 Mozilla 사이트에 잘 남겨져 있다. [ [바로가기](https://developer.mozilla.org/ko/docs/Web/HTTP/Status) ]

웹페이지는 여러 객체로 이루어져있는데 한 개의 페이지는 다수의 HTTP 트랜잭션을 요청하는 경우가 일반적이다. 이는 웹페이지가 여러 리소스로 되어있다는 걸 의미한다.

![HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.18.17.png](HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.18.17.png)

[ 예제 프로젝트의 index.html 페이지에서 여러 트랜잭션이 발생하는걸 보여주는 모습 ]

### 4. 메시지

HTTP 트랜잭션에는 요청과 응답 메시지가 있는데 이는 단순한 줄 단위의 문자열이다. HTTP 메시지는 세 부분으로 이루어지는데 시작 줄, 헤더, 본문이 그러한 내용이다.

![HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.33.28.png](HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.33.28.png)

[ HTTP Request Message ]

Request 메시지의 경우 본문이 없고 시작줄과 헤더로 되어있다.

![HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.31.57.png](HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.31.57.png)

[ Http Response Message의 Header ]

서버에서 보내주는 Response Message 헤더를 보면 HTTP 버전과, 응답 코드로 시작줄이 있고 그리고 헤더 정보가 들어가있다.

![HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.36.35.png](HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/_2021-01-17__7.36.35.png)

[ Http Response Message의 Response ]

index.html을 서버로 요청한 URI 였기 때문에 노드 서버의 index.html 을 응답한 걸 알 수 있다.

### 6. TCP 커넥션

HTTP는 애플리케이션 계층 프로토콜이다. 네트워크 프로토콜은 여러 계층으로 되어있는데 HTTP는 네트워크 통신에 세부적인 내용은 신경쓰지 않는다. 대신 중요한 네트워크 연결은 TCP/IP에 맡긴다. 

TCP/IP는 전송 계층에서 아래와 같은 내용을 보장하는데

- 오류 없는 데이터 전송
- 순서에 맞는 데이터 전달
- 조각나지 않는 데이터 스트림

을 제공하게 된다.  이렇기 때문에 HTTP2까지는 TCP를 사용해왔지만 HTTP3 부터는 UDP를 사용해 통신하게 된다. 이에 대한 내용은 나중에 다룬다.

![HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/1_tnEkvHfXNnhv7xAthT2sJQ.png](HTTP%20%E1%84%80%E1%85%B5%E1%84%8E%E1%85%A9%20d0292984ff6e4df3931ad2bbbbb39de5/1_tnEkvHfXNnhv7xAthT2sJQ.png)

[ TCP/IP 모델과 OSI 7계층 ]

웹 브라우저와 서버가 http 통신을 하는 과정은 아래와 같다.

1. 웹브라우저가 URL에서 호스트 명을 추출한다.
2. 웹브라우저가 호스트명을 IP로 변환한다.
3. URL의 기본 포트는 80이지만, 포트가 있다면 추출한다.
4. 이제 웹 서버와 TCP 커넥션을 맺는다.
5. 서버에 HTTP 요청을 보낸다.
6. 서버가 HTTP 응답을 돌려준다.
7. TCP 커넥션을 닫고, 웹브라우저가 응답을 보여준다.

여기서 중요한 점은 HTTP 는 요청과 응답만 받고, 실제 통신은 TCP 가 한다는 점이다.

### 7. 터널

이제는 https가 권장되는 (크롬에서 https가 우선적으로 요청되도록 테스트 하고 있다.  [chromium issue tracker](https://bugs.chromium.org/p/chromium/issues/detail?id=1141691)) 환경이니 이제 https 는 상식이 되었다고 생각한다. (제발 https 사용합시다. 국가 인증서는 사용하지 말고)

터널은 HTTP 어플리케이션 중 하나인데, 두 커넥션 사이 Raw 데이터를 열어보지 않고 전달해 주는 역할을 한다. (보안) HTTPS 에 대한 내용은 다음에 다루도록 하고, HTTP 터널의 대표적인 예시가 HTTP/SSL라는 점을 알고 넘어가면 된다.