# 1. 운영체제(Operating System)

운영체제는 컴퓨터 시스템의 각종 자원을 효율적으로 관리하고 컴퓨터 HW(CPU, I/O)와 User간의 인터페이스를 담당하는 시스템 프로그램압니다.

- 프로그램들이 자원을 필요로 할 때 할당함
- 각각 자원을 할당받은 프로그램들이 서로 엉키지 않도록 제어함
- 하드웨어(HW)와 사용자간의 인터페이스를 담당

운영체제의 종류로는 MacOS, Linux, Windows, Android, iOS가 대표적입니다.

## 01. 운영체제 부팅과정

운영체제 역시 소프트웨어(SW)이므로 컴퓨터의 전원이 켜지면 메모리에 운영체제를 적재합니다. 이 과정에서 ROM에 저장되어 있는 `부트로더(Boot Loader)`에 의해서 적재됩니다.
![](https://velog.velcdn.com/images/jgone2/post/d2842f6e-fd0a-41e1-bafd-d72071facb02/image.png)


- 1. 컴퓨터의 전원이 On 상태가 되면 `POST(Power-On Self Test)`를 실행하여 컴퓨터에 연결되어 있는 HW(I/O 등)의 연결 상태가 정상인지 확인
- 2. OS는 비휘발성 저장장치인 보조기억장치에 저장되어 있으므로, ROM의 부트로더를 통해서 보조기억장치에 있는 OS에 접근
- 3. 보조기억장치에 있는 OS를 RAM에 적재해서 실행
- 4. OS가 Processor에 관여하며, 컴퓨터가 종료될 때까지 RAM에 적재상태를 유지

> 💡 **부트로더(Boot Loader)**
운영체제가 시동되기 이전에 미리 실행되어 커널이 올바르게 시동되기 위해 필요한 모든 관련 작업을 마무리하고, 최종적으로 운영체제를 시동시키기 위한 목적을 가진 프로그램

## 02. 운영체제의 역할

- 프로세스 관리(Process Management)
- 메모리 관리(Memory Management)
- 저장소 관리(Storage Management)
- 보안(Protection and Security)

# 2. 운영체제 작동방식

DOS와 같은 초기의 운영체제는 하나의 CPU에서 하나의 프로그램만 실행되는 단일 프로그래밍 시스템에서 현재의 빠르고 효율적인 시스템의 운영체제로 발전되어왔습니다.

## 01. 일괄처리 시스템(Batch System)

![](https://media.vlpt.us/images/fredkeemhaus/post/94e5f549-9260-487c-a786-cbfaa41017b7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-25%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%201.40.00.png)

- 유사한 요구를 가지는 작업을 모아 하나의 그룹으로 수행하는 시스템
- 시스템 지향적(System-Oriented)
- 모든 시스템을 중앙에서 관리 및 운영
- 사용자의 요청 작업을 일정 시간 모아두었다가 한번에 처리
- 하나의 CPU로 하나의 작업만 처리 할 수 있어서, 하나의 작업이 끝날 때까지 다른 작업은 대기

### 장점

- 많은 사용자가 시스템 자원을 공유 할 수 있음
- 처리 효율(throughput)향상

### 단점

- 생산성 저하(유사한 작업 요청들을 일정시간 모았다가 한번에 처리하기 때문)
- 긴 응답시간


## 02. 다중 프로그램 시스템(Multi-Programmed System)

![](https://media.vlpt.us/images/fredkeemhaus/post/ac9feae7-68e4-44bf-af47-c0c5a0bf148f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-25%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%201.43.15.png)

- 처리해야할 프로세스가 있을 시 수행할 작업을 항상 가지도록 하는 시스템
- A프로세스에서 입출력을 수행중일 때, B작업에 CPU를 할당해서 처리하는 방식
- A프로세스에 CPU를 할당해서 작업중인 경우, I/O 장치들을 대기 상태로 만들어서 입출력이 필요한 작업에게 입출력 리소스를 할당

## 03. 시분할 시스템(Time-Sharing System)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fl1AhX%2Fbtq4HIgXRnb%2F5LQRUNKCB9icCpKeGRXRPk%2Fimg.png)

- 아주 짧은 주기로 CPU를 각각의 프로그램에 할당
- 모든 프로그램이 동시에 작용하고 있다고 느끼게 함(사용자 입장)

### 장점

- 응답시간(response time) 단축
- 생산성 향상(프로세서 유휴 시간 감소)

### 단점

- 통신 비용 증가(단말기를 통해 접속하기 때문)
  - 통신선 비용, 보안문제 등
- 개인 사용자 체감 속도 저하 가능성
  - (동시 사용자수 ↑) → (시스템 부하 ↑) → (느려짐)

## 04. 실시간 처리 시스템(Real-Time System)

- 작업 처리에 제한시간(deadLine)을 갖는 시스템
- 즉시적인 처리를 요할 때 사용
- 제한 시간 내 서비스를 제공하는 것이 자원 활용 효율보다 중요할 때 사용
- Hard Real-Time System과 Soft Real-Time System나뉨

### 01. Hard Real-Time System

- 시간 제약을 무조건 지켜야 하는 시스템
- 지키지 못하는 경우 시스템에 치명적 영향
- 발전소 제어, 무기 제어 같은 엄격한 프로그램

### 02. Soft Real-Time System

- 동영상 재생 등

## 05. 병렬 처리 시스템(Parallel Processing System)

![](https://media.vlpt.us/images/fredkeemhaus/post/d19db7e9-f23d-4614-b495-7a8b7953a143/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-25%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%201.52.54.png)

- 단일 시스템 내에서 둘 이상의 프로세서 사용
  - 동시에 둘 이상의 프로세스 지원
- 메모리 등의 자원 공유(Tightly-Coupled System, 강결합 시스템)
  - 동일 운영체제 하에서 여러개의 프로세스가 하나의 메모리를 공유
  - 하나의 운영체제가 모든 프로세스와 시스템 하드웨어 제어
  - 결합력이 강함
  - 공유 메모리를 차지하려는 프로세스 간의 경쟁 최소화 필요

### 사용목적

- 성능 향상
- 신뢰성 향상
  - 부품중 하나가 고장나더라도 다른 부품이 프로세스를 넘겨받아 처리할 수 있어 정상 동작 가능

## 06. 분산 처리 시스템(Distributed Processing System)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBB22D%2Fbtq4ILYt3xh%2FdfcfVLRqwohAYdhaKyAcIk%2Fimg.jpg)


- 네트워크를 기반으로 구축된 병렬처리기반 시스템(Loosely-Coupled System, 약결합 시스템)

  - 각 프로세스마다 독립된 메모리를 가진 시스템
  - 둘 이상의 독립된 컴퓨터 시스템을 네트워크를 이용해 연결한 시스템
  - 각 시스템마다 독자적인 운영체제 가지고 있음
  - 각 시스템은 독립적으로 작동, 필요한 경우 상호 통신도 할 수 있음
  - 프로세스끼리의 통신은 메시지 전달이나 원격 `프로시저` 호출을 통해 이루어진다.
  - 각 시스템마다 독자적인 운영이 가능하므로 결합력 약함

> 💡 **프로시저**  
호출을 통해 실행되어 미리 저장해 놓은 SQL작업을 수행하는 것으로,  
다시말해 '어떤 업무를 수행하기 위한 절차'를 뜻한다.

- 독립적인 처리 능력을 가진 컴퓨터 시스템을 네트워크를 이용해서 연결해 작업을 처리하는 시스템
- CPU들은 메모리들을 공휴하지 않음.

### 장점

- 자원 공유를 통한 높은 성능
- 신뢰성이 높음
- 높은 확장성

## 단점

- 구축 및 관리가 어려움

## 07. 약결합 시스템과 강결합 시스템(Loosely-Coupled System & Tightly-Coupled System)

| 구분     |             약결합             |           강결합            |
| -------- | :----------------------------: | :-------------------------: |
| 메모리   |              독립              |            공유             |
| 운영체제 |             독자적             | 하나의 운영체제에 의한 제어 |
| 통신     | 메시지 전달&원격 프로시저 호출 |         공유 메모리         |
| 결합력   |              약함              |            강함             |

<br />

# 3. 운영체제 동작원리

- 운영체제는 interrupt(event)-driven방식으로 작동됨
- interrupt-driven방식은 사용자의 요청(interrupt 혹은 event)이 발생하면 운영체제는 적절하게 자원(CPU, I/O, Memory 등)을 분배하여 요청을 처리하는 구조
- interrupt에는 두가지 종류가 있음
  - H/W interrupt
  - S/W interrupt

## 01. 인터럽트(Interrupt)

인터럽트는 운영체제가 어떤 작업을 수행하고 있을 때, 하드웨어나 프로그램으로부터 받는 새로운 신호를 말합니다. 사전적 의미처럼 `방해`의 의미로, 작업 수행 중 인터럽트 신호를 받게 되면 진행중인 작업을 일시 중단하고 인터럽트 신호에 포함된 작업을 먼저 수행하게 됩니다.

1. 인터럽트가 발생되면 OS는 다음에 실행할 명령어를 스택(stack)에 저장
2. 발생된 인터럽트를 수행
3. 인터럽트가 종료되면 스택에 저장된 명령어 주소로 돌아가서 다음 명령어를 수행하거나 대기상태 복귀

## 02. H/W interrupt

- 하드웨어가 발생시키는 인터럽트로, CPU가 아닌 다른 하드웨어 장치가<br /> cpu에 어떤 사실을 알려주거나 cpu 서비스를 요청해야 할 경우 발생

## 03. S/W interrupt

- 소프트웨어가 발생시키는 인터럽트
- 소프트웨어(사용자 프로그램)가 스스로 인터럽트 라인을 세팅
  - 종류: 예외 상황, system call
- 인터럽트를 발생시키기 위해 하드웨어/소프트웨어는 cpu내에 있는 인터럽트 라인을 세팅하여 인터럽트를 발생시킴
- cpu는 매번 명령을 수행하기 전에 인터럽트라인이 세팅되어있는지를 검사


# 📚 Reference

- [ROM의 Boot Loader](https://vividswan.github.io/2020/11/01/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-ROM%EC%9D%98-Boot-Loader.html)
- [운영체제의 구조와 원리](https://velog.io/@brian_kim/OS-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B5%AC%EC%A1%B0%EC%99%80-%EC%9B%90%EB%A6%AC)
- [OS의 기본적인 작동 방식](https://choirim.tistory.com/65)
- [운영체제의 구조 및 동작원리](https://baked-corn.tistory.com/3)
- [[정의]Loosely Coupled vs. Tightly Coupled](https://akawarren.tistory.com/27)
- [인터럽트, 예외, 트랩의 비교](http://melonicedlatte.com/computerarchitecture/2019/02/12/213856.html)
- [인터럽트 제대로 이해하기](https://velog.io/@adam2/%EC%9D%B8%ED%84%B0%EB%9F%BD%ED%8A%B8)
- [OS Overview](https://velog.io/@fredkeemhaus/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-Lecture-2.-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%97%AD%ED%95%A0#%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C%EC%9D%98-%EA%B5%AC%EB%B6%84)
