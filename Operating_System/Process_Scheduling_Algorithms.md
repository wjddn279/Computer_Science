## Process Scheduling Algorithms

## FCFS (First-Come-First-Servie)

- Non-preemptive scheduling (비 선점 스케줄링)
- 스케줄링 기준 
  - 도착 시간 (ready queue 기준) -> 쉽게 말해 선착순
  - 먼저 도착한 프로세스를 먼저 처리
- 자원을 효율적으로 사용 가능
  - 스케줄링에 대한 오버헤드가 낮다
  - CPU가 멈춤없이 일 할 수 있다.
- Batch system에 적합, interactive system 에 부적합
- 단점
  - Convoy effect
    - 하나의 수행시간이 긴 프로세스에 의해 다른 프로세스들이 긴 대기시간을 갖게 됨
    - 대기시간 >> 실행시간
  - 긴 평균 응답 시간(response time)

![image-20200919123309255](.\images\image-20200919123309255.png)

- example

![image-20200919124301554](.\images\image-20200919124301554.png)

- Normalized TT : Turn around time/ Burst time
  - 사용하기 위해서 얼마나 기다려야 하는지 상대적으로 나타냄
  - 1분 사용하기 위해 1분 기다리는 것과 1시간 사용하기 위해 1분 기다리는 것의 의미 차이를 해소하기 위해 사용

## Round Robin (RR)

- Preemptive scheduling
- 스케줄링 기준
  - 도착 시간(ready queue)
  - 먼저 도착한 프로세스를 먼저 처리
- **자원 사용 제한 시간(time quantum)이 있음**
  - system parameter
  - 프로세스는 할당된 시간이 지나면 자원 반납 (timer-runout)
  - 특정 프로세스의 자원 독점 방지
  - Context swithch overhead가 큼 -> 프로세스가 계속 바뀌기 때문에
- 대화형(interactive system), 시분할(time sharing system) 시스템에 적합
- Time quantum 이 시스템 성능을 결정하는 핵심 요소
  - Very large (infinite) -> FCFS와 같음
  - Very small time quantum -> processor sharing
    - 사용자는 모든 프로세스가 각각의 프로세서 위에서 실행되는 것처럼 느낌
      - 체감 프로세서 속도 = 실제 프로세서 성능의 1/n
      - high context swithching overhead

![image-20200919125000983](.\images\image-20200919125000983.png)

- 한번 processor에서 실행되다가 time-out으로 쫓겨난다면 ready queue의 맨 뒤로 가야함

- example

time quantum = 2s

![image-20200919125912088](.\images\image-20200919125912088.png)

- p1 (0 도착, 3 burst time)
  - 맨 처음에 2초(time quantum) 동안 일하다가 쫓겨남 (context saving을 상태를 통해 PCB에 저장)
  - ready queue 맨 뒤로 갔다가 다시 순서가 오면 나머지 1초 해결하고 exit

- 나머지 : 2초 이상이면 2초 동안 일하고 PCB에 현재 상태 context saving 하고 ready queue 맨 뒤로 감
- 자기 차례가 오면 다시 2초 동안 일하고 다 하면 exit 다못하면 또 ready 저장

time quantum : 3s 

![image-20200919130051753](C:\Users\multicampus\Desktop\면접 내용 정리\Operating_System\images\image-20200919130051753.png)

## Shortest Process Next (SPN)

- Non-preemptive scheduling
- 스케줄링 기준
  - 실행시간(burst time 기준)
  - ready queue에 존재하는 burst time 가장 작은 프로세스를 먼저 처리
    - SJF (Shortest job First) scheduling
- 장점
  - 평균 대기시간 최소화
  - 시스템 내 프로세스 수 최소화
    - 스케줄링 부하 감소, 메모리 절약 -> 시스템 효율 향상
  - 많은 프로세스들에게 빠른 응답 시간 제공
- 단점
  - Starvation (무한대기) 현상 발생
    - BT가 긴 프로세스는 자원을 할당 받지 못 할 수 있음
    - ex) bt가 10이라서 대기하고 있는데 1~2짜리가 계속 들어오면 10짜리는 처리할 수 없게 됨
      - Aging 등으로 해결
  - 정확한 실행시간을 알 수 없음
    - 실행시간 예측 기법이 필요
- example

![image-20200919130853839](.\images\image-20200919130853839.png)

## Shortest Remaining Time Next (SRTN)

- SPN의 변형
- Preemptive scheduling
  - 잔여 실행 시간이 더 적은 프로세스가 ready 상태(등장)가 되면 선점 됨
- 장점
  - SPN의 장점 극대화
- 단점
  - 프로세스 생성시, 총 실행 시간 예측이 필요함
  - 잔여 실행을 계속 추적해야 함 = overhead
  - Context switching overhead

## High Response Ratio Next (HRRN)

- Non-preemtive scheduling
- SPN의 변형
  - SPN + Aging concepts, Non-preemptive scheduling
- Aging concepts
  - 프로세스의 대기 시간(WT)을 고려하여 기회를 제공 (Statvation 방지)
- 스케줄링 기준
  - Response ratio가 높은 프로세스 우선
- Response ratio = (WT+BT)/BT (응답률) -> 필요한 BT 대비 얼마나 기다렸는가?
  - SPN의 장점 + Starvation 방지
  - 실행 시간 예측 기법 필요 (overhead)

- example

![image-20200919131532070](.\images\image-20200919131532070.png)

## Summary

![image-20200919131747196](.\images\image-20200919131747196.png)

- FCFS, RR : process간 공평성을 보장하지만 효율성 성능이 떨어짐
- SPN -> SRTN -> HRRN 으로 공평성, 효율,성능 모두를 얻기 위해 개선이 이어짐
- 하지만 실행시간 예측의 정확성 문제, Overhead 문제가 있음