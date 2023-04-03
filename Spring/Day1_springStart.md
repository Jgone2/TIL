# 1. Spring Framework 시작하기

## 스프링 프로젝트 생성

- [spring initializr](https://start.spring.io/)
  ![](https://velog.velcdn.com/images/jgone2/post/ab54f93b-17a1-4c34-ae65-a89157aeb651/image.png)

### 1. Project

- **Gradle**: 최근 추세는 Gradle을 많이 사용한다고 함. default값
- **Maven**: 레거시나 과거 프로젝트의 경우 Maven으로 생성되어 있을 수 있음.

### 2. Spring Boot

- 개발 시에는 안정적으로 서비스를 운영할 수 있도록 `LTS(Long Term Support)`버전을 사용해야 함.
- LTS는 장기간에 걸쳐 지원하는 소프트웨어 버전.
- '3.0.0'버전은 Java 17버전부터 사용이 가능.

### 3. Project Metadata

- **Group**: 보통 기업명이나 소속을 작성
- **Artifact**: 프로젝트명(Name과 Artifact는 동일하게 설정됨)

### 4. Dependencies

프로젝트에서 사용할 `라이브러리(Library)`를 선택할 수 있다. Dependencies에서 손쉽게 라이브러리를 사용할 수 있음.

- **Spring Web**: Web Application을 개발하는데 필요한 의존 라이브러리들을 자동 생성
- **Lombok**: Annotation을 통해 자주 사용하는 Java코드를 자동으로 구성해주는 라이브러리
- **Thymeleaf**: HTML을 만들어주는 Template Engine

### 5. Open Project

- 1. 아래쪽에 표시 한 `GENERATE`를 클릭하면 압축된 프로젝트 파일을 다운로드함.
- 2. 다운받은 프로젝트 파일의 압축을 해제
- 3. 압축 해제한 폴더 내에 build.gradle을 `Open as Project`로 open

# 2. lombok Setting

`lombok`은 사용하기 위해서 사전설정을 해줘야합니다.

- IntelliJ 상단 메뉴에서 [File] → [Settings] 열기
  ![](https://velog.velcdn.com/images/jgone2/post/1e727472-2d8d-4483-9919-1bdd1bf77780/image.png)
- `Annotation`을 검색
- `Annotation Processors`에서 `Enable annotation processing` 체크 ✔️

# 3. Application 동작 확인

![](https://velog.velcdn.com/images/jgone2/post/2f0e5c05-f9fb-43d4-97fa-7c64c53f32be/image.png)
`Artifact+Application`으로 이루어진 파일을 실행시키고 정상 실행 여부 확인.

# 📚 Reference

- [Infrean - 김영한님 스프링 로드맵](https://www.inflearn.com/roadmaps/373)
- [코드스테이츠 백엔드 부트캠프] (https://www.codestates.com/course/backend-engineering)
