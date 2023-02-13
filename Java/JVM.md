![Java](https://velog.velcdn.com/images/jgone2/post/173cda11-caf7-4289-97fb-7112c586eb1d/image.png)

# JVM(Java Virtual Machine)

JVM은 '**J**ava **V**irtual **M**achine'의 줄임말로 자바를 실행하기 위한 가상머신(컴퓨터)이다.  
컴퓨터를 사용해서 Java를 실행하기 위한 가상의 컴퓨터라고 생각하면 된다. 즉, 컴퓨터 속의 컴퓨터이다.

<br />

# JVM 장점

### 1. OS에 종속받지 않는다.

![일반 애플리케이션과 Java 애플리케이션의 차이](https://velog.velcdn.com/images/jgone2/post/c685ee87-e276-4115-a35c-ecfcef441505/image.jpeg)

Java는 OS에 종속받지 않고 어느 OS나 실행가능하다는 특징을 가지고 있다. OS에 종속받지 않을 수 있는 이유가 JVM이 있기 때문이다. 다만 JVM은 OS와 직접 소통하기때문에 각 OS규격에 맞는 JVM이 필요하다.  
하지만 Java를 개발한 썬 마이크로시스템즈(Sun Microsystems, Inc.)에서 이미 주요 OS용 JVM을 제공하고 있기 때문에 걱정할 필요가 없다.

### 2. 자동 메모리관리(feat. Garbage Collection)

Java 이전에는 프로그래머가 모든 프로그램의 메모리를 관리했어야 했다. 그러나 자바는 JVM이 프로그램 메모리를 대신 관리해준다. 가비지 컬렉션(Garbage Collection. 이하 GC)프로세스를 통해 메모리를 관리하며, 가비지 컬렉션은 자바 프로그램에서 사용되지 않는 메모리를 지속적으로 찾아내서 제거해준다.

<br />

> 💡 자바는 실행 시에 해석되기 때문에 속도가 느리다는 단점을 가지고 있지만 최근엔 바이트코드(Compiled Java Code)를 하드웨어의 기계어로 바로 변환해주는 `JIT컴파일러`와 최적화 기술로 많이 빨라졌다.

# Java 컴파일 과정

![](https://velog.velcdn.com/images/jgone2/post/4ad73b96-6722-4478-81d9-8d30cdb041e4/image.jpeg)
Java코드(`*.java`)는 CPU가 인식을 하지 못하므로 기계어로 컴파일하는 과정이 필요하다. 하지만 JVM은 사용자 언어인 원시코드(`*.java`)를 인식하지 못한다. 그래서 JVM으로 해석하기 전에 Compiler를 통해 JVM이 인식할 수 있는 Java ByteCode(`*.class`)로 변환하는 과정을 거친다
변환된 ByteCode도 OS에서 이해할 수 있는 기계어가 아니기 때문에 `JVM`이 OS가 이해할 수 있도록 'ByteCode -> 기계어'로 해석해준다.
자바코드가 이런 과정으로 컴파일 되기 때문에 "Write once, run anywhere.(한 번 작성하면 어디서든 실행된다."이 가능하게 된다.

# JVM 구성요소

![JVM 구성 요소](https://velog.velcdn.com/images/jgone2/post/be50f82c-d97e-4f7f-80ba-14595fd4004e/image.png)

JVM은 크게 다음과 같이 구성되어 있다.

- 클래스 로더(Class Loader)
  - 로드(Loading)
    - 링크(Linking)
    - 초기화(Initialization)
- 실행 엔진(Execution Engine)
  - 인터프리터(interpreter)
  - JIT(Just-In-Time) 컴파일러
  - 가비지 콜렉터(Garbage Collector)
- 런타임 데이터 영역(Runtime Data Area)

## 클래스 로더(Class Loader)

자바는 동적 로드를 한다. 이 말은 컴파일 타임이 아니라 런타임에 클래스를 처음으로 참조할 때 해당 클래스를 로드하고 링크한다는 말이다. 이 동적 로드를 담당하는 부분이 클래스 로더(Class Loader)다.
![](https://velog.velcdn.com/images/jgone2/post/ac898659-0728-41e4-82e2-22bfdf540f17/image.jpeg)
클래식 로더가 로드되지 않은 클래스를 찾으면 다음과 같은 과정을 거쳐 처리한다.

- **로드(Loading)**: 클래스(`.*class`)를 파일에서 가져와서 JVM의 메모리에 로드.
- **링크(Linking)**
  - **검증(Verifying)**: 로드된 클래스 파일을 자바 언어 명세(Java Language Specification) 및 JVM 명세에 명시된 대로 구성되어 있는지 검사한다.
  - **준비(Preparing)**: 클래스가 필요로 하는 메모리를 할당하고 정의된 필드, 메서드, 인터페이스들을 나타내는 데이터 구조를 준비한다.
  - **분석(Resolving)**: 클래스의 상수 풀 내 모든 심볼릭 래퍼런스를 다이렉트 레퍼런스로 변경한다.
- **초기화(Initializing)**: 클래스 변수들을 적절한(미리 설정된) 값으로 초기화한다.

## 런타임 데이터 영역(Runtime Data Area)

![](https://velog.velcdn.com/images/jgone2/post/2c4e50b7-a758-4288-8159-434fd9da4360/image.png)

런타임 데이터 영역은 JVM이라는 프로그램이 운영체제 위에서 실행되면서 할당받는 메모리 영역이다.

- **PC 레지스터(PC Register)**: 각 스레드(Thread)마다 하나씩 존재하며 스레드가 시작될 때 생성된다. 현재 수행 중인 JVM 명령의 주소를 갖는다.
- **JVM stack**: 각 스레드마다 하나씩 존재하며 스레드가 시작될 때 생성된다. 스택 프레임(Stack Frame)이라는 구조체를 저장하는 스택으로, 오직 JVM 스택에 스택 프레임을 추가(push)하고 제거(pop)하는 동작만 수행한다.
  스택에 할당된 값들은 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 소멸되는 특성을 가지고 있다.
- **Native Method Stack**: 자바 외의 언어로 작성된 기계어로 작성된 코드를 위한 스택으로 해당 언어에 맞게 독자적으로 스택이 생성된다.

> 💡 스레드(Thread)

- 하나의 프로세스 안에서 실행되는 흐름의 단위.
  모든 프로세스에는 한 개 이상의 스레드가 포함되어 있다. 두 개 이상의 스레드를 가지는 프로세스를 멀티 스레드 프로세스(Multi Thread Process)라고 한다.

> 💡 프로세스(Process)

- 현재 실행중인 프로그램.
  프로그램 파일이 주기억장치(RAM)에 적재되어 CPU에 의해 실행 중인 것을 말한다.

## 실행 엔진(Execution Engine)

클래스 로더를 통해 JVM 내의 런타임 데이터 영역에 배치된 ByteCode는 실행 엔진에 의해 실행된다. 실행 엔진은 자바 바이트코드를 명령어 단위로 읽어서 실행한다.

- **인터프리터**: 자바 바이트코드 명령어를 하나씩 읽어서 해석하고 실행한다. 한 줄씩 해석하고 실행하기 때문에 하나하나의 해석은 빠르지만 인터프리팅 결과의 실행은 느리다는 단점을 보유.
- **JIT(Just-In-Time) 컴파일러**: JIT컴파일러는 인터프리터의 단점을 보완하기 위해 도입됐다. 인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 컴파일하여 기계어로 변환한다. 이후에 기계어를 `캐시(Cache)`에 보관하기 때문에 한 번 컴파일된 코드를 계속 빠르게 직접 실행할 수 있다.
- **가비지 콜렉터(GC)**: 위에서 언급했듯이 사용되지 않는 인스턴스를 찾아내서 메모리에서 제거한다.

# JDK와 JRE의 차이

---

Method Area, Heap 영역 등은 좀 더 공부를 한 후 추가하도록 하겠다.

# 📚 Reference

- [Seize the day!](https://doozi0316.tistory.com/entry/1%EC%A3%BC%EC%B0%A8-JVM%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%93%9C%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B8%EA%B0%80)
- ["JVM이란 무엇인가"자바 가상 머신 이해하기](https://www.itworld.co.kr/news/110837)
- [NAVER D2 - JVM Internal](https://d2.naver.com/helloworld/1230)
