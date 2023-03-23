# 1.브라우저
- 대표적인 브라우저 서비스로 **크롬, 파이어폭스, 사파리, 오페라, 엣지, 웨일**이 존재
<br /><br />

# 2. 브라우저의 기능
- 필요한 **데이터를 서버에 요청하고 브라우저에 표시**
- 데이터의 주소는 **URI**(Uniform Resource Identifier)에 의해 정해짐
- HTML 및 CSS를 해석해서 브라우저에 표시하는데, 웹 표준화 기구인 에서 **[W3C](https://www.w3.org/)에서 정한 기준**에 따름
>💡 브라우저의 UI는 주소 표시줄, 상태 표시줄, 도구 모음 등 일반적인 요소를 제외하고는 표준 명세가 정의된 바 없지만, 수 년간 서로의 장점을 모방하면서 서로 비슷한 현재에 이르게 되었다.

<br />

# 3. 브라우저 기본구조
![브라우저 기본구조](https://d2.naver.com/content/images/2015/06/helloworld-59361-1.png)<br />
브라우저의 주요 구성 요소의 역할 다음과 같다.<br /><br />
### 01. 사용자 인터페이스(UI)
주소 표시줄, 이전/다음 버튼, 북마크 메뉴 등. 요청한 페이지를 보여주는 창을 제외한 나머지 모든 부분
### 02. 브라우저 엔진
UI와 렌더링 엔진 사이의 동작을 제어하는 역할
### 03. 렌더링 엔진
HTML 및 XML문서와 CSS를 파싱(분석)하여 요청한 콘텐츠를 화면에 표시해주는 역할
>💡 XML(Extensible Markup Language)는 문자 기반의 마크업 언어로 데이터를 저장하고 전달할 목적으로 만들어졌으며, 태그(<>)를 사용하여 데이터의 의미를 정의하는 프로세스이다.
### 04. 통신
HTTP 요청과 같은 네트워크 호출에 사용되며, 독립적인 인터페이스로 각 플랫폼 하부에서 실행됨
### 05. UI 백엔드
[콤보박스](https://ko.wikipedia.org/wiki/%EC%BD%A4%EB%B3%B4_%EC%83%81%EC%9E%90)(Combo Box)와 같은 그래픽 사용자 인터페이스 위젯과 화면 상의 기본적인 장치를 그리는 것 
### 06. 자바스크립트 해석기
자바스크립트 코드를 파싱하고 실행하는 역할
### 07. 자료 저장소
쿠키나 세션과 같은 로컬 데이터를 저장하는 역할

# 4. 렌더링 엔진
렌더링 엔진은 HTML과 CSS뿐만 아니라 플러그인 혹은 브라우저 확장 기능을 이용해 PDF 등 다른 유형의 데이터도 표시 가능
## 01. 렌더링 엔진의 종류
렌더링 엔진의 종류에는 모질라(Mozila)에서 제작한 **게코**(Gecko)엔진과 오픈소스 엔진인 **[웹킷](https://webkit.org/)**(Webkit), 웹킷에서 파생된 **블링크**(Blink) 등이 존재<br />
- `Gecko` - 파이어 폭스
- `Webkit` - 사파리
- `Blink` - 크롬
<br /><br />

## 02. 작동방식
렌더링 엔진은 서버에 요청한 문서를 응답받는 것으로 시작되며, 보통 8KB단위로 전송됨<br />
↓렌더링 엔진의 기본 동작과정은 다음 과 같다.<br />
![렌더링 엔진 기본 동작 과정](https://d2.naver.com/content/images/2015/06/helloworld-59361-2.png)<br />
### 01. HTML 파싱 및 DOM 노드 변환
HTML 문서를 파싱하여 콘텐츠 내부의 각 태그들을 DOM에서 하나하나의 **노드로 변환**
### 02. 렌더 트리 구축
CSS파일과 함께 포함도니 스타일 요소들을 파싱한 후, 스타일정보와 DOM노드를 표시규칙에 따라 렌더 트리를 구축
### 03. 렌더 트리 배치
생성된 렌더 트리를 화면에 배치함으로써, 각 노드가 화면의정확한 위치에 표시될 수 있도록 함
### 04. 렌더 트리 그리기
배치 후 UI 백엔드에서 렌더 트리의 각 노드의 형상을 그리는 과정
### 05. 표시
UI 백엔드에서 그리기 과정이 끝나면 결과를 화면에 출력

> ### DOM 추가 설명
> DOM은 Document of Object Model의 준말로 해석하면 `문서 객체 모델`이다.<br />
![DOM](https://media.geeksforgeeks.org/wp-content/uploads/20210908120846/DOM.png)<br />
위와 같은 문서 객체 모델을 가진다고 하면 HTML문서는
```
<!DOCTYPE html>
<html>
<head>
  <title>My title</title>
</head>
<body>
  <h1>A heading</h1>
  <a href="javascript:void(0);">Link Text</a>
</body>
</html>
```
의 형태를 가지고 있을 것이다.
>💡 href의 javascript:void(0);은 아무 작업이 실행되지 않음을 의미한다.

# 📚 Reference
[Naver D2 - 브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)<br />
[뺑슨 개발 블로그 - 웹 브라우저는 어떻게 작동하는가?](https://bbangson.tistory.com/87)<br />
[[웹개발] 브라우저의 작동 원리](https://it-ist.tistory.com/110)<br />
[tcpschool](http://www.tcpschool.com/webbasic/works)<br />
[URI & URL](https://velog.io/@jch9537/URI-URL)<br />
[브라우저와 렌더링 엔진](https://feel5ny.github.io/2018/05/29/rendering_engine_0/)<br />
[How WebRender gets rid of jank](https://hacks.mozilla.org/2017/10/the-whole-web-at-maximum-fps-how-webrender-gets-rid-of-jank/)<br />