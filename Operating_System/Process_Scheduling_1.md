## 다중프로그래밍

- 여러개의 프로세스가 시스템 내 존재 (동시에 실행 되고 있음)
- 자원을 할당 할 프로세스를 선택 해야 함
  - 스케줄링
- 자원 관리
  - 시간 분할 (time sharing) 관리
    - 하나의 자원을 여러 스레드(자원을 공유하니까) 들이 번갈아 가며 사용
    - ex) 프로세서
    - 프로세스 스케줄링
  - 공간 분할 (space sharing) 관리
    - 하나의 자원을 분할하여 동시에 사용
    - ex) 메모리

## 스케줄링의 목적

- 스케줄링 -> 어떤 프로세스를 프로세서에 올릴것인가 정하는 것

- 시스템의 성능 향상
- 대표적 시스템 성능 지표
  - 응답 시간 (response time) -> real time system(제한된 시간), iteractive system(대화형)
    - 작업 요청으로부터 응답을 받을때까지의 시간
  - 작업 처리량 (throughput) ->batch system(일괄처리)
    - 단위 시간 동안 완료된 작업의 수
  - 자원 활용도 (resource utilization) -> 비싼 장비를 활용할 떄
    - 주어진 시간 동안 자원이 활용된 시간
- **목적에 맞는 지표를 고려하여 스케줄링 기법을 선택**
- 평균 응답시간, 처리량, 자원활용도, 공평성, 실행 대기 방지, 예측 가능성

## 대기시간, 응답시간, 반환시간

![image-20200919105414475](images\image-20200919105414475.png)

- 대기시간 : 도착 ~ 실행 시작
- 응답시간 : 도착 ~ 첫번째 출력(응답)
- 실행시간: 실행 ~ 실행 종료
- 반환시간: 도착 ~ 실행 종료(총 시간)

## 스케줄링 기준

- 스케줄링 기법이 고려하는 항목들 -> 어떤 프로세스를 먼저 할당할지 정하는 기준
- 프로세스(process)의 특성
  - I/O bounded or compute-bounded
- 시스템 특성(목적)
  - batch system or interactive system
- 프로세스의 긴급성
  - Hard or Soft real time (real time system), non real time system
- 프로세스 우선순위
- 프로세스 총 실행 시간

## CPU burst vs I/O burst

- 프로세스 수행 = CPU 사용(연산) + I/O 대기(사용)
- CPU burst:  프로세스의 CPU 사용 시간
- I/O burst : 프로세스의 I/O 대기 시간
- CPU burst > I/O busrt : compute bounded (CPU의 사용 시간을 길게 사용하는 프로세스)
- CPU burst < I/O busrt : I/O bounded (I/O 대기 시간을 길게 사용하는 프로세스)

## 스케줄링의 단계 (Level)

- 발생하는 빈도 및 할당 자원에 따른 구분

- Long-term scheduling (장기 스케줄링) ->  가끔 스케줄링이 발생

  - Job scheduling (Long-term scheduling의 일종)
    - 시스템에 제출 할(Kernel에 등록 할) 작업 결정
    - 시스템에 job을 올릴건데 뭐 부터 올릴건지 얼마나 올릴건지 결정
  - 다중 프로그래밍 정도 조절
    - 시스템 내에 프로세스 수 조절
  - I/O bounded 와 compute-bounded 프로세스들을 잘 섞어서 선택하야 함
    - why? 
  - 시분할 시스템에서는 모든 작업을 시스템에 등록
    - Long-term scheduling이 불필요 -> 어차피 다 올리니까

- Mid-term scheduling 

  - 메모리 할당 결정 (memory allocation)
    - Swapping (ready <-> suspended ready)
    - -> suspended ready 상태의 어떤 프로세스에게 메모리를 주어 ready로 만들지 결정

  

![image-20200919111239367](images\image-20200919111239367.png)

- Short-term Scheduling 
  - Process Scheduling (Short-term Scheduling의 일종)
    - 프로세스를 처리할 프로세서를 결정하는 스케줄링
    - Low-level scheduling 
    - 프로세서를 할당할 프로세스를 결정
      - Processor scheduler, dispatcher
  - 가장 빈번하게 발생
    - Interrupt, block, time-out, Etc
    - 매우 빨라야 함
      - average CPU burst = 100ms
      - scheduling decision = 10ms
      - 10 * (100+10) = 9%의 CPU가 스케줄링을 위해 사용 -> 느릴수록 오버헤드의 증가

![image-20200919111844248](images\image-20200919111844248.png)

- 스케줄링의 단계

![image-20200919111936904](images\image-20200919111936904.png)

## 스케줄링 정책 (Policy)

- 선점 vs 비선점
  - Preemptive scheduling, Non-preemptive scheduling
- 우선순위
  - priority

## Preemptive, Non-preemptive scheduling

- Non-preemptive scheduling (비선점 -> 뻇을 수 없다)
  - 할당 받을 자원을 스스로 반납할 때 까지 사용
    - system call
  - 장점
    - context switch overhead 가 적음
  - 단점
    - 잦은 우선순위 역전, 평균 응답 시간 증가
- Preemptive scheduling(선점 -> 뺏을 수 있다)
  - 타의에 의해 자원을 빼앗길 수 있음
    - 예) 할당시간 종료, 우선순위가 높은 프로스세 등장
  - context switch overhead 가 큼
  - time-sharing system, real-time system 등에 적합 (응답성 높음)

## Priority

- 프로세스 중요도
- static priority (정적 우선순위)
  - 프로세스 생성시 결정된 priority가 유지 됨
  - 구현이 쉽고, overhead가 적음
  - 시스템 환경 변화에 대한 대응이 어려움
- dynamic priority (동적 우선순위)
  - 프로세스의 상태 변화에 따라 priority 변경
  - 구현이 복잡, priority 재계산 overhead가 큼
  - 시스템 환경 변화에 유연한 대응 가능