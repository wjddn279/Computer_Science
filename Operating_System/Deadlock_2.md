## Deadlock 발생의 예

- 2개의 프로세스 (P1,P2)
- 2개의 자원 (R1,R2)

![image-20200928101655931](.\images\image-20200928101655931.png)

- t1 ~ t4 : P1은 자원 R2 점유, P2는 자원 R1 점유
- t4 ~ : p1는 R1 요구, P2는 R2 요구 -> 교착 상태

## Graph Model

- Node 
  - 프로세스 노드(P1,P2), 자원 노드(R1,R2)
- Edge
  - Rj -> Pi : 자원 Rj이 프로세스 Pi에 할당 됨
  - Pi -> Rj : 프로세스 Pi가 자원 Rj을 요청 (대기 중)

![image-20200928103038457](.\images\image-20200928103038457.png)

- 프로세스, 자원간의 cycle 생성 -> Deadlock

## State Transition Model

- 예제
  - 2개의 프로세스와 A type의 자원 2개(unit) 존재
  - 프로세스는 한번에 자원 하나만 요청/반납 가능
- State

![image-20200928103258061](.\images\image-20200928103258061.png)

![image-20200928103458128](.\images\image-20200928103458128.png)

- P1 이 자원을 점유하고 자원을 요구 함과 동시에 P2도 마찬가지라면 deadlock

## Deadlock 발생 필요 조건

- Exclusive use of resources - 자원의 특성
- Non-preemptible resources - 자원의 특성
- Hold and wailt (Partial allocation) - 프로세스의 특성
  - 자원을 하나 hold 하고 다른 자원 요청
- Circular wait - 프로세스의 특성

- 4개의 조건 모두 만족 해야 deadlock이 발생함