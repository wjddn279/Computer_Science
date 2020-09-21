# Mutual Exclusion Solutions

- 상호 배제 문제 해결 하기 위한 방안

## Dekker's Algorithm

- Two process ME을 보장하는 최초의 알고리즘

![image-20200920234134481](.\images\image-20200920234134481.png)

- 기존의 문제를 해결하기 위해 Flag 와 turn의 개념을 합쳐 문제를 해결

- Algorithm 최적화

![image-20200920234554229](.\images\image-20200920234554229.png)

## Dijkstra's Algorithm

- N-Process Mutual Exclusion 문제 해결
- Dijkstra 알고리즘의 flag 변수
  - idle : 프로세스가 cs(critical section) 임계지역 진입을 시도하고 있지 않을 때
  - want-in : 프로세스가 cs 진입 시도 1단계 일 때
  - in-CS : 프로세스의 임계 지역 지입 시도 2단계 및 임계지역 내에 있을 때 

![image-20200920234931161](.\images\image-20200920234931161.png)

- Step 1.
  - 내가 들어가고 싶다고 의사표현 (flag[i] <- want-in)
  - turn이 내 차례가 아니면 올때 까지 기다림
  - 현재 실행중인 process가 끝나면 turn을 뺏어옴
- Step 2.
  - inCS 상태에 한명만 있도록 check 해줌
  - in-CS 상태의 process가 한개이면 cs로 진입하고 아니면 모두 0단계로 돌아간다.

## SW solutions

- ex) Dekker's algorithm, dijkstra algorithm
- SW solution들의 문제점
  - 속도가 느림
  - 구현이 복잡함
  - ME primitive 실행 중 preemption 될 수 있음
    - 공유 데이터 수정 중은 interrupt를 억제함으로서 해결 가능
      - overhead 발생
  - Busy waiting
    - 비효울적

## HW solution

- TestAndSet (TAS) instruction 
  - Test 와 Set을 한번에 수행하는 기계어
  - Machine Instruction
    - Atomicity, indivisible
    - 실행 중 interrupt를 받지 않음(preemption 되지 않음)
  - Busy waiting
    - Inefficient

![image-20200921000222241](.\images\image-20200921000222241.png)

![image-20200921000352629](.\images\image-20200921000352629.png)

- TAS : 현재의 값을 return 하고 그 값은 True로 만들어 준다
- while (TAS(lock)) do -> lock이 true 이면 계속 cs에 접근 못하고 돌다가
- cs안에 있는 프로세스가 끝나면 lock이 false가 되면서 while 문이 깨지고 다시 들어감
- 하드웨어로 인한 한번에 실행이 됨이 보장 됨으로써 사용 가능

- But 3개 이상의 프로세스의 경우, Bounded waiting 조건 위배
  - 