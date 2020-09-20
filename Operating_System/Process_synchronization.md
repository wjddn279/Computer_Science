# Process Synchronization (동기화)

- 다중 프로그래밍 시스템
  - 여러 개의 프로세스들이 존재 (동시에 여러 프로세스 동작)
  - 프로세스들은 서로 독립적으로 동작
  - 공유 자원 또는 데이터가 있을 떄, 문제 발생 가능 (동시에 자원에 접근하면)
- 동기화 (Synchronization)
  - 프로세스 들이 서로 동작을 맞추는 것 -> 서로간의 대화를 하는 것
  - 프로세스 들이 서로 정보를 공유하는 것

## Asynchronous and Concurrent P's

- 비동기적 (Asynchronous)

  - 프로세스들이 서로에 대해 모름

- 병행적 (Concurrent)

  - 여러 개의 프로세스들이 동시에 시스템에 존재 (동시에 동작)

- 병행 수행중인 비동기적 프로세스들이 공유 자원에 동시 접근 할 때 문제가 발생 할 수 있음

  -> 서로간의 정보가 없기 때문에 같은 자원을 동시에 점유하려는 접근이 생길 수 있음

## Terminologies

- Shared data(공유 데이터) or critical data
  - 여러 프로세스들이 공유하는 데이터
- Critical section(임계 영역)
  - 공유 데이터를 접근하는 코드 영역 (code segment)
- Mutual exclusion (상호 배제)
  - 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것

## Critical Section

![image-20200920231026447](.\images\image-20200920231026447.png)

- sdata : shared data(critcal data) 공유 데이터
- 기계어 명령 (machine instruction)의 특성
  - Atomicity, indivisible(분리불가능)
  - 한 기계의 명령의 실행 도중에 인터럽트 받지 않음

![image-20200920231443080](.\images\image-20200920231443080.png)

- machine instruction 으로 변역된 형태

![image-20200920231655390](.\images\image-20200920231655390.png)

- 실행 순서에 따라 같은 연산의 결과가 달라지는 현상 -> Race Condition

##  Mutual Exclusion (상호 배제)

- 앞의 예 에서는 1,2,3 이 실행되는 도중 A,B,C가 실행 되는 것을 막아줌 
  - 즉 Critical Section에 하나가 들어가면 다른 것은 못 들어오게 막는 것

![image-20200920232023163](.\images\image-20200920232023163.png)

- Mutual exclusion primitive (primitive -> 기본이 되는 연산)
  - enterCS()  primitive  (CS: Critical Section)
    - Critical Section 진입 전 검사
    - 다른 프로세스가 critical section 안에 있는지 검사
  - exitCS() primitive
    - Critical section을 벗어날 때의 후처리 과정
    - Critical section을 벗어남을 시스템이 알림

- Requirements for ME primitive (필요조건)
  - Mutual exclusion (상호배제)
    - Critical section (CS)에 프로세스가 있으면, 다른 프로세스의 진입을 금지
  - Progress (진행)
    - CS 안에 있는 프로세스 외에는, 다른 프로세스가 CS에 진입하는 것을 방해하면 안됨
    - CS 안에 프로세스가 있어서 못들어가는 경우를 제외하곤 다 들어 갈 수 있어야 함
  - Bounded waiting (한정대기)
    - 프로세스의 cs진입은 유한시간 내에 허용되어야 함

## Two process mutual exclusion

- ME primitives version 1

![image-20200920232745031](.\images\image-20200920232745031.png)

- turn을 나눠서 내 차례가 되면 들어 갈 수 있다.	
- 상대방이 들어가면 turn 이 바뀜
- P0 : turn == 1 이라면 기다리고 turn -- 0 이면 cs에 진입
- 나오면 turn = 1 로 만들고 넘겨 줌
- 세가지 필요 조건 검증 (Mutual exclusive, Progress, Bounded waiting)
  - Progress 조건 위배 
    - why? -> P0이 CS에 진입하지 않는 경우 턴이 바뀌지 않아 P1도 접근 불가
    - 한 Process가 두번 연속 CS에 진입 불가 (턴이 바뀌지 않으므로)
- ME primitives version 2

![image-20200920233343725](.\images\image-20200920233343725.png)

- Flag의 사용
- 현재 내가 들어가면 True 나오면 False
- 상대방의 Flag가 True이면 들어갈 수 있고 False 이면 못들어감
- 세가지 필요 조건 검증 (Mutual exclusive, Progress, Bounded waiting)
  - 한명이 이미 들어가 있고 Preemption 되면 Flag가 바뀌지 않아 둘 다 들어갈 수 있다
  - Mutual exclusion 조건 위배
- ME primitives version 3

![image-20200920233800769](.\images\image-20200920233800769.png)

- 들어갈 것이라 의사를 밝히고 나머지가 없으면 들어감
- 세가지 필요 조건 검증 (Mutual exclusive, Progress, Bounded waiting)
  - Progress, Bounded waiting 조건 위배