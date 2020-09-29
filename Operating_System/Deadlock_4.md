## Deadlock detection and deadlock recovery methods

- Deadlock 방지를 위한 사전 작업을 하지 않음
  - Deadlock이 발생 가능
- 주기적으로 Deadlock 발생 확인
  - 시스템이 deadlock 상태인가?
  - 어떤 프로세스가 deadlock 인가?
- Resource Allocation Graph (RAG) 사용

# Deadlock Detection

- Resource Allocation Graph (RAG)
  - Deadlock 검출을 위해 사용
  - Directed ( 방향성이 있는) , Bipartite(두개의 파트로 나뉜 -> 프로세스와 자원) Graph

![image-20200928202013428](.\images\image-20200928202013428.png)

- Resource Allocation Graph (RAG)

  - Directed graph G = (N,E)

    - Node : N  = {Np, Nr} where
      - Np is the set of proccess = {P1,P2,..,Pn}
      - and Nr is the set of resources = {R1,R2,...,Rm}

    ![image-20200928202425719](.\images\image-20200928202425719.png)

  - processor 와 resource 두개의 집합 (Bipartitle graph)

    - Edge : Np 와 Nr 사이에만 존재
      - e = (Pi, Rj) : 자원 요청
      - e = (Rj, Pi) : 자원 할당

![image-20200928202530570](.\images\image-20200928202530570.png)

- Notations 
  - Rk : k type 의 자원
  - Tk : Rk의 단위 자원 수
  - |(a,b)| : (a,b) edge 의 수
- RAG example

![image-20200928202902869](.\images\image-20200928202902869.png)

## Deadlock Detection Method

## Graph reduction

- Graph reduction
  - 주어진 RAG에서 edge를 하니씩 지워가는 방법
  - Completely reduced (다 지워짐 -> deadlock 아님)
    - 모든 edge가 제거 됨
    - Deadlock에 빠진 프로세스가 없음
  - Irreducible (다 안지워 진다 -> deadlock)
    - 지울 수 없는 edge가 존재
    - 하나 이상의 프로세스가 deadlock 상태

- Unblocked process
  - 필요한 자원을 모두 할당 받을 수 있는 프로세스

![image-20200928203146976](.\images\image-20200928203146976.png)

- 좌변 : Pi 가 요청하는 모든 자원 수
- 우변 : 전체 갯수 - 사용중 -> 남은 수
- 요청하는 자원 수 <= 남은 자원의   수 -> 필요한 자원을 모두 할당 받을 수 있는 프로세스.

### Graph reduction procedure

1. Unblocked process에 연결 된 모든 edge를 제거
2. 더 이상 unblocked process가 없을 때 까지 1. 반복

- 최종 graph에서
  - 모든 edge가 제거 됨 (Completely reduced)
    - 현재 상태에서 deadlock이 없음
  - 일부 edge가 남음 (irreducible)   -> **자원을 할당받을 수 없는 프로세스가 존재**
    - 현재 deadlock이 존재

### Graph Reduction (example 1)

![image-20200928204205250](.\images\image-20200928204205250.png)

- (a) 에서 P1이 unblocked 상태 이므로 edge P1의 간선 제거
- (b) 에서 P2이 unblocked 상태 이므로 edge P2의 간선 제거
- (c) 에서 남은 edge가 없으므로 deadlock 상태가 아님

### Graph Reduction (example 2)

![image-20200928204501025](.\images\image-20200928204501025.png)

- P3 말고는 unblocked 상태가 아님 -> deadlock 상태

### Graph Reduction Summary

- High overhead
  - 검사 주기에 영향을 받음
  - Node의 수가 많은 경우
- Low overhead deadlock detection methods (special case)
  - Case-1) Single-unit resources
    - Cycle detection
  - Case-2) Single-unit request in expedient state
  - 

### Deadlock Avoidance vs Detection

- Deadlock avoidance
  - 최악의 경우를 생각
    - 앞으로 일어날 일을 고려
  - Deadlock이 발생하지 않음
- Deadlock detection
  - 최선의 경우를 생각
    - 현재 상태만 고려
  - Deadlock 발생 시 Recovery 과정이 필요

## Deadlock Recovery

- Process termination
  - Deadlock 상태인 프로세스 중 일부 종료
- Termination cost model
  - 종료 시킬 deadlock 상태의 프로세스 선택
  - Termination cost
    - 우선순위 
    - 종류
    - 총 수행시간
    - 남은 수행 시간
    - 종료 비용
    - Etc

### Process termination

- 종료 프로세스 선택
  - Lowest-termination cost process first -> 종료시 기준 비용이 가장 적은 프로세스를 종료
    - simple
    - Low overhead
    - 불필요한 프로세스들이 종료 될 가능성이 높음
  - Minimum cost recovery -> 일일히 하면서 최소의 비용을 가지는 프로세스 종료
    - 최소 비용으로 deadlock 상태를 해소 할 수 있는 process 선택
      - 모든 경우의 수를 고려해야 함
    - Complex
    - High overhead
      - O(2^k)

### Resource preemption

- Deadlock 상태 해결을 위해 선점할 자원 선택
- 해당 자원을 가지고 있는 프로세스를 종료 시킴
  - Deadlock 상태가 아닌 프로세스가 종료 될 수도 있음
  - 해당 프로세스는 이후 재시작 됨
- 선점할 자원 선택 
  - Preemption cost model 이 필요
  - Minimum cost recovery method 사용
    - O(r)

### Checkpount-restart method

- 프로세스의 수행 중 특정 지점(checkpoint) 마다 context를 저장
- Rollback을 위해 사용
  - 프로세스 강제 종료 후, 가장 최근의 checkpoint에서 재시작

![image-20200928205628543](.\images\image-20200928205628543.png\