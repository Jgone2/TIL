# URI(Uniform Resource Identifier)

- URI는 URL과 URN을 포함
  - URI는 Locator, Name 또는 둘 다 추가로 분류 가능
  - URN은 잘 사용하지 않음

### URI 뜻

- Uniform: 리소스 식별하는 통일된 방식
- Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- Identifier: 다른 항목과 구분하는데 필요한 정보
  <br /><br />
- URL(Locator): 리소스가 있는 위치를 지정
- URN(Name): 리소스에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않음
- URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음
- URI와 URL로 기억할 것

# 문법

- scheme://[userinfo@]host[:post][/path][?query][#fragment]<br />
- https://www.google.com:443/search?q=hello&hi=ko
  - 프로토콜(https)
  - 호스트명(www.google.com)
  - 포트 번호(443)
  - 패스(/search)
  - 쿼리 파라미터(q=hello&hi=ko)<br /><br />

### Scheme

- 주로 프로토콜 사용
- 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
  - Ex) http, https, ftp emd
- http는 `80 포트`, https는 `443 포트`를 주로 사용, 포트는 생략 가능
- https는 http에 보안 추가(HTTP Secure)

### Userinfo

- URL에 사용자 정보를 포함해서 인증
- 거의 사용하지 않음

### Host

- 호스트명
- 도메인명 또는 IP 주로슬 직접 사용가능

### Port

-접속 포트 -일반적으로 생략

### Path

- 리소스 경로, 계층적 구조
  - Ex) /home/file1.jpg, /items/iphone11

### Query

- key = value 형태
- ?로 시작, &으로 추가 가능 ?key1=value1&key2=value2
- query parameter, query string 등으로 불림
- 웹서버에 제공하는 파라미터
- 문자 형태

### Fragment

- html 내부 북마크 등에 사용
- 서버에 전송하는 정보 아님

# 웹 브라우저 요청 흐름

1. 웹 브라우저가 HTTP 메시지 생성

```query
GET /search?q=hello&hi=ko HTTP/1.1
Host: www.google.com
```

2. SOCKET 라이브러리를 통해 전달
  - A: TCP/IP 연결(IP, PORT)
  - B: 데이터 전달

3. TCP/IP 패킷 생성, HTTP 메시지 포함

4. 요청 패킷 도착

5. 응답 패킷 전달
  ```query
  HTTP/1.1 200 OK
  Content-Type: text.html;charset=UTF-8
  Content-Length: 3423

  <html>
    <body>...</body>
  </html>
  ```

6. 응답 패킷 도착 후 렌더링

# 📚Reference

- [CodeStates](https://www.codestates.com/course/backend-engineering?gclid=Cj0KCQjw2v-gBhC1ARIsAOQdKY2Hj9THw6YromW_L-Dmtq7eQ_z47MDN-chkF-_YKrqCkJEhtPOnmngaAlumEALw_wcB)
- [모든 개발자를 위한 HTTP 기본 지식(김영한님-Infrean)](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)