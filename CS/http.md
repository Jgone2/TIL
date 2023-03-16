# 1.HTTP란

HTTP(HypterText Transfer Protocol)는 HTML 문서와 같은 리소스들을 서버와 클라이언트 사이에서 정보를 교환할 수 있도록 해주는 프로토콜이다. HTTP는 웹에서 사용된다.

> 💡프로토콜은 컴퓨터끼리의 데이터 교환 방식을 정의하는 규칙 체계들의 집합을 말한다. 간단히 말해서 상호 간에 정의한 규칙을 의미한다.

<br />

# 2. HTTP 역사

- HTTP/0.9(1991): GET Method만 지원, HTTP 헤더 X
- HTTP/1.0(1996): Method, 헤더 추가
- HTTP/1.1(1997): 현재 가장 많이 사용.
  - RFC2068(1997) -> RFC2616(1999) -> RFC7230~7235(2014)
- HTTP/2(2015): 성능 개선
- HTTP/3: TCP 대신에 UDP 사용, 성능 개선

<br />

# 3.HTTP의 특징

- 클라이언트 서버 구조
  - HTTP 메세지는 **서버와 클라이언트에 의해 해석**된다.
- 무상태 프로토콜(stateless), 비연결성
  - HTTP는 연결상태를 유지하지 않는 **비연결성 프로토콜**이다.
    →이는 클라이언트가 이전에 요청한 내용을 기억하고 있지 않다는 것을 의미한다.
  - 비연결성 프로토콜이므로 **요청/응답(Request/Response)** 방식으로 작동한다.
  - 또한 **stateless**한 프로토콜이므로, 각각의 데이터 요청이 **서로 독립적으로 관리**가 된다. 즉, 이전 데이터 요청과 다음 데이터 요청이 서로 관련이 없다.
  - **다수의 요청 처리 및 서버의 부하를 줄일 수 있는 성능 상의 이점**이 있다.
  - 그러나 비연결성이고 상태가 없기 때문에, 통신할 때마다 세로운 커넥션을 만들어야 하므로 **클라이언트는 계속적인 인증이 필요하다는 단점**이 있다.
    이 단점을 보완하기 위해 **쿠키(Cookie)와 세션(Session)**을 사용한다.
- 단순함, 확장 용이성
  - HTTP/1.0에서 소개된 HTTP헤더 덕분에 **HTTP를 확장**하고 실험하기 쉬워졌다.
- HTTP 프로토콜은 일반적으로 **TCP/IP 통신을 이용하며, 기본 포트는 80**이다.

> 💡HTTP의 기본 포트번호가 80인 이유는 HTTP가 문서화 되기 전부터 사용하지 않는 빈 포트 번호였고, 이 번호가 1991년 HTTP 0.9 버전에서 처음으로 문서화되면서 기본 포트로 지정되었기 때문이다.

<br />

### 01. 클라이언트 서버 구조

- Request Response(요청 응답) 구조
- 클라이언트는 서버에 요청을 보내고, 응답 대기
- 서버가 요청에 대한 결과를 응답

### 02. 무상태 프로토콜(Stateless)

- 서버가 클라이언트의 상태 보존 X
- 장점: 서버 확장성 높음(Scale Out)
- 단점: 클라이언트가 추가 데이터 전송 필요

### 03. Stateful과 Stateless

- Stateful
  - 항상 같은 서버 유지
- Stateless
  - 아무 서버나 호출 가능
  - 수평 확장 유리
    - 정말 같은 시간에 딱 맞춰 발생하는 대용량 트래픽에 사용
      - Ex) 선착순 이벤트, 명절 기차 예매, 수강신청 등
  - 실무적 한계 존재
    - 모든 것을 무상태로 설계 할 수 있는 경우도 있고 없는 경우도 있다.
      - Ex) 로그인
  - 일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태 유지
  - 상태 유지는 최소한만 사용
  - 전송해야할 데이터가 비교적 많음

### 04. 비 연결성

- HTTP는 Default가 연결을 유지하지 않는 모델
- 일반적으로 초 단위 이하의 빠른 속도로 응답
- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 적음
- 서버 자원을 매우 효율적으로 사용 가능
  <br />

- 한계
  - 매 번 새로 연결해야함.
  - 웹 브라우저로 사이트를 요청하면 HTML에 링크된 수많은 자원이 함께 다운로드
  - 현재는 HTTP 지속 연결(Persistent Connections)로 문제 해결
  - HTTP/2, HTTP/3에서 더 많은 최적화

# 3.요청과 응답(Request & Response)

HTTP 프로토콜로 데이터를 주고받기 위해서는 아래와 같이 **요청(Request)을 보내고 응답(Response)** 을 받아야한다.<br />

![](https://images.velog.io/images/jgone2/post/026cbcbb-0e1a-4858-b795-b8254ffc48e2/image.png)<br />

> 💡요청과 응답은 **클라이언트(Client)와 서버(Server)사이**에서 이루어 진다.
> 클라이언트는 요청을 보내는 쪽이며, 웹에서는 **브라우저**가 이에 해당되고, 서버는 요청을 받고 응답하는 쪽이며 일반적으로** 데이터를 보내주는 원격지의 컴퓨터**를 의미한다.

## 01.요청(Request)

요청을 할 때는 URL을 이용하면 서버에 특정 데이터를 요청할 수 있다.
요청을 수행할 때는 HTTP 요청 메서드를(HTTP Request Methods)를 사용한다.
HTTP 요청 메서드는 HTTP Verbs라고도 불리며, 아래와 같은 주요 메서드를 가지고 있다.

### 주요 메서드

- **GET**: 리소스 **조회, 요청**
- **POST:** 요청 데이터 **처리**, 새로운 리소스 **생성, 등록**
- **PUT**: 리소스 **전체 변경/수정/대체**, 해당 리소스가 없으면 생성
- **PATCH**: 리소스 **일부 변경/수정**(Update와 비슷하게 쓰인다.)
- **DELETE**: 리소스 **삭제**

### 기타 메서드

- **HEAD**: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
- **OPTIONS**: 대상 리소스에 대한 서버에서 지원하는 메소드 확인. [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)에서 사용된다.
- **CONNECT**: 대상 자원으로 식별되는 서버에 대한 터널 설정
- **TRACE**: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트 수행

HTTP 요청 메서드에서도 영역을 나눌 수 있다.

### 01. Request Line

먼저 위에서 언급된 **URL**과, **요청 메서드**, 그리고 **HTTP 버전정보**가 포함되어있으며, 요청 메시지의 첫째 줄에 표기된다.

### 02. Request Header

두 번째 줄은 헤더 영역이다. 헤더에는 클라이언트 PC와 브라우저 정보, 쿠키 등 다양한 **클라이언트 환경에 대한 정보**를 가지고 있다.
User-Agent, Upgrade-Insecure-Requests, Accept-Language등 여러가지 종류가 해당된다.

### 03. CRLF

줄바꿈 명령으로 한 줄을 띄운다.

### 04. 본문[ message-body ]

클라이언트가 입력한 데이터를 저장하는 영역이다.

```
GET https://velog.io/@jgone2 HTTP/1.1		          // 시작줄
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) ... // 헤더
Upgrade-Insecure-Requests: 1
```

<br />

## 02.응답(Response)

응답은 HTTP 프로토콜의 버전, 상태코드, 상태 메시지, Response Header, 본문으로 구성되어 있다.<br />
![](https://images.velog.io/images/jgone2/post/4dcc85be-d18a-448a-9f53-38df6131df08/HTTP_Response.png)<br />

### 01.HTTP Status code(상태 코드)

`상태 코드`는 서버가 클라이언트에게 응답의 상태를 알리는 수단이며, 크게 **다섯가지** 클래스로 분류된다.

- 1xx: 요청 정보 처리중(Informational)
- `2xx`: 요청을 정상적으로 처리(Success)
- 3xx: 요청을 완료하기 위해 추가 수행 필요(Redirection)
- `4xx`: 요청한 자원이 서버에 존재하지 않음(Client Error)
- 5xx: 서버가 요청 처리 실패(Server Error)
  <br /><br />

# 📚Reference

[프런트엔드 개발자가 알아야하는 HTTP 프로토콜](https://joshua1988.github.io/web-development/http-part1/)<br />
[http의 기본 포트가 80인 이유는 무엇일까?](https://johngrib.github.io/wiki/why-http-80-https-443/)<br />
[HTTP란 무엇인가?](https://velog.io/@surim014/HTTP%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)<br />
[[웹 기본개념] HTTP 통신](https://jinbroing.tistory.com/63)<br />
[HTTP 개념잡기 통신과정](https://velog.io/@doomchit_3/Internet-HTTP-%EA%B0%9C%EB%85%90%EC%B0%A8%EB%A0%B7-IMBETPY)<br />
[Mozilla](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)<br />
[TCP SCHOOL](http://tcpschool.com/webbasic/address)<br />
[모든 개발자를 위한 HTTP 기본 지식(김영한님-Infrean)](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
