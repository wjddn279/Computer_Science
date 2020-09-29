# Deadlock 발생 필요 조건

- Exclusive use of resources 혼자 쓰는 자원
- Non-preemptible resources 선점 당하지 않는 자원
- Hold and wait (Partial allocation) 
- Circular wait

## Deadlock 해결 방법

## Deadlock Prevention (예방)

- 4가지 필요조건 중 하나를 절대 발생하지 않도록 막는 방법
- 4개의 deadlock 발생 필요조건 중 하나를 제거
- Deadlock이 절대 발생하지 않음
- 심각한 자원 낭비 발생
  - Low device utilization
  - Reduced system throughput
- 비현실적

1. 모든 자원을 공유 허용

   - Exclusive use of resource 조건 제거

   - 현실적으로 불가능 !!

2. 모든 자원에 대해 선점 허용
   - Non-preemptible resoures 조건 제거
   - 현실적으로 불가능
   - 유사한 방법
     - 프로세스가 할당 받을 수 없는 자원을 요청한 경우, 기존에 가지고 있던 자원을 모두 반납하고 작업 취소
       - 이후 처음(또는 check-point) 부터 다시 시작
     - 심각한 자원 낭비 발생 -> 비현실적 

3. 필요 자원 한번에 모두 할당 (total allocation)
   - Hold and wait 조건 제거
   - 프로세스가 시작될 때 필요한 자원을 한꺼번에 가지고 독점하고 있는 방법
   - 자원 낭비 발생
     - 필요하지 않은 순간에도 가지고 있음
     - 다른 필요한 프로세스들이 사용하지 못해 대기현상이 길어짐
   - 무한 대기 현상 발생 가능

4. Circular wait 조건 제거
   - Totally allocation을 일반화 한 방법
   - 자원들에게 순서를 부여
   - 프로세스는 순서의 증가 방향으로만 자원 요청 가능
   - 자원 낭비 발생

![image-20200928105725560](images\image-20200928105725560.png)

- 무조건 순서대로 P1은 1,2,3,4 순서대로 자원을, P2는 1,3 순서대로 증가하는 방향으로 사용
- 순환 구조가 생기지 않음
- 하지만 P2 입장에서 3번을 먼저 사용할 수 있지만 1번을 먼저 사용해야 함으로 대기해야함

## Deadlock Avoidance (회피)

- 시스템의 상태를 계속 감시
- 시스템이 deadlock 상태가 될 가능성이 있는 자원 할당 요청 보류
- 시스템을 항상 safe state로 유지
- Safe state
  - 모든 프로세스가 정상적으로 종료 가능한 상태
  - safe sequence가 존재
    - Deadlock 상태가 되지 않을 수 있음을 보장
- Unsafe state
  - Deadlock 상태가 될 가능성이 있음
  - 반드시 발생한다는 의미는 아님
- 가정 (Avoidance를 하기 위한 가정) -> 현실적이지 않음
  - 프로세스의 수가 고정됨
  - 자원의 종류와 수가 고정됨
  - 프로세스가 요구하는 자원 및 최대 수량을 알고 있음
  - 프로세스는 자원을 사용 후 반드시 반납한다

## Deadlock Avoidance 알고리즘

1. Dijkstra's algorithm ( Banker's algorithm  은행원 알고리즘)
   - Deadlock avoidance를 위한 간단한 이론적 기법
   - 가정
     - 한 종류(resource type)의 자원이 여러 개(unit)
   - 시스템을 항상 safe state로 유지

## Safe state example

- 1 resource type R(자원), 10 resource units, 3 processes

![image-20200928175918299](images\image-20200928175918299.png)

- Max.Claim (최대 자원 필요수), Cur.Alloc (현재 할당 된 자원 수)
-  Additional Need = Max.Claim - Cur.Alloc (최대 더 필요한 자원 수)

- Avaliable resource units : 2
- 순서대로 Available resource units을 프로세스에 할당할 것인데 어떠한 순서대로 할당 해야 할까?
- 실행 종료 순서 : P1 -> P3 -> P2 (Safe sequence)
- 맨 처음 P1에게 할당해서 끝나면 사용 가능한 자원이 3개(원래거 2개 + P1이 가지고 있던 자원 1개)가 된다
- 그 다음 P3에게 할당하여 끝나면 사용 가능한 자원이 5개 그것을 P2에게 할당 후 마무리
- 현재 상태에서 안전 순서(Safe sequence)가 하나이상 존재하면 안전 상태(Safe state)임 

## Unsafe state examle

![image-20200928182028894](images\image-20200928182028894.png)

- Available resource units : 2
- No safe sequence
- 임의의 순간에 세 프로세스들이 모두 세 개 이상의 단위 자원을 요청하는 경우 시스템은 교착상태에 놓이게 됨
- Unsafe state

## Djikstra's banker's algorithm (Example)

- 1 resource type R, 10 resource units, 3 processes (2개의 자원 사용 가능)

![image-20200928183317416](images\image-20200928183317416.png)

- P1 이 1개의 자원을 요청했다 -> 빌려줬다고 가정하고 시뮬레이션
- P1을 한개 빌려줬을 때, 그 후 Safe state가 존재하는지 확인하는 시뮬레이션

- Safe state가 존재한다면 자원 할당을 승인하고 자원 할당

![image-20200928183530349](images\image-20200928183530349.png)

- P2가 1개의 자원을 요청했다 -> 빌려줬다고 가정하고 시뮬레이션
- Safe state가 아니기 때문에 (Unsafe state) 요청을 거절함.

**Banker's algorithm : 시뮬레이션 해보고 safe sequence 가 존재한다면 할당 아니면 거부**

## Harbermann's algorithm

- Dijkstra's algorithm의 확장
- 여러 종류의 자원 고려
  - Multiple resource types
  - Multiple resource units for each resource type
- 시스템을 항상 safe state로 유지

## Harbermann's algorithm (Example)

- 3 types of resources: Ra, Rb, Rc
- Number of resource units for each type : (10,5,7)
- 5 process

![image-20200928184313357](images\image-20200928184313357.png)

- Available resource units : 전체 자원 갯수(Number of resource units for each type) :(10,5,7)

  ​											  현재 사용되고 있는 자원 갯수 Ra,Rb,Rc 각각 (7,2,5)

  ​												사용 가능한 자원 갯수 (3,3,2)

- Safe sequence가 존재하므로 Safe state

![image-20200928184455298](images\image-20200928184455298.png)

- P2 에서 (1,0,2) 요청 -> 시뮬레이션 결과 Safe sequence 존재
- Safe state 이므로 할당 허가

![image-20200928184619734](images\image-20200928184619734.png)

- P1 에서 (0,3,0) 자원 할당 요청 -> 시뮬레이션 결과 Safe sequence 없음
- Unsafe state 이므로 할당 거부

## Deadlock avoidance Summary

- Deadlock 의 발생을 막을 수 있음
- High overhead 
  - 항상 시스템을 감시하고 있어야 한다.
- Low resource utilization
  - 자원 효율성 낮음
  - Safe state 유지를 위해, 사용 되지 않는 자원이 존재
  - **최악의 Unsafe state지만 실제로 아닌경우에도 할당이 거부 됨으로 자원 효율성 낮음**
- Not practical
  - 가정
    - 프로세스 수, 자원 수가 고정.
    - 필요한 최대 자원 수를 알고 있음.