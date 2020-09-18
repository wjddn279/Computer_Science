# 프로세스 관리

- Job vs Process
  - 작업 : 실행 할 프로그램 + 데이터, 실행 요청 전의 상태
  - 디스크에 적재 되어 있는 프로그램
- 프로세스
  - 실행을 위해 시스템(커널)에 등록된 작업
  - 시스템 성능 향상을 위해 커널에 의해 관리 됨
  - 실행 되기 위해 메모리에 적재 된 프로그램(job) -> 프로세스

![image-20200918101405614](.\images\image-20200918101405614.png)

## 프로세스의 정의

- 실행중인 프로그램
  - 커널에 등록되고 커널의 관리하에 있는 작업
  - 각종 자원들을 요청하고 할당 받을 수 있는 개체
  - 프로세스 관리 블록(PCB)을 할당 받은 개체
  - 능동적인 개체 (실행중에 각종 자원을 요구,할당,반납하며 진행)

![image-20200918101641351](.\images\image-20200918101641351.png)

## 자원(Resource)의 개념

- 커널의 관리 하에 프로세스에게 할당/반납 되는 수동적 개체(passive entity)
- 자원의 분류
  - H/W resources
    - processor,memory,disk,monitor ..
  - S/W resources
    - message,signal,files ..

## Process Control Block (PCB)

- OS가 프로세스 관리에 필요한 정보 저장
- 프로세스 생성 시, 생성 됨
- 커널에 존재

![image-20200918101942582](.\images\image-20200918101942582.png)

- PCB가 관리하는 정보

  - PID : Proceess identification Number
    - 프로세스 고유 식별 번호
  - 스케줄링 정보
    - 프로세스 우선순위 등과 같은 스케줄링 관련 정보들
  - 프로세스 상태
    - 자원 할당, 요청 정보 등
  - 메모리 관리 정보
    - Page table, segment table 등
  - 입출력 상태 정보
    - 할당 받은 입출력 장치, 파일 등에 대한 정보 등
  - 문맥 저장 영역
    - 프로세스의 레지스터 상태를 저장하는 공간 등
  - 계정 정보
    - 자원 사용 시간 등을 관리

  **PCB 정보는 OS 별로 서로 다름**

  **PCB 참조 및 갱신 속도는 OS의 성능을 결정 짓는 중요한 요소 중 하나**

## 프로세스의 상태 (Process State)

- 프로세스 - 자원 간의 상호 작용에 의해 결정

1. Created State
   - 작업을 커널에 등록
   - PCB 할당 및 프로세스 생성
   - 커널
     - 가용 메모리 공간 체크 및 프로세스 상태 전이
     - 메모리가 있다? -> ready
     - 메모리가 없다? -> suspended ready

![image-20200918102651746](.\images\image-20200918102651746.png)

2. Ready State
   - 프로세서 외에 다른 모든 자원을 할당 받은 상태
     - 프로세서 할당 대기 상태
     - 즉시 실행 가능 상태
   - Dispatch (or Schedule)
     - Ready state - Running state

![image-20200918102900980](.\images\image-20200918102900980.png)

3. Runnig State
   - 프로세서와 필요한 자원을 모두 할당 받은 상태
   - Preemiption
     - Runnig state -> ready states
     - 프로세서 스케줄링에 따라 할당된 자원을 뺏기고 ready 상태로 내려가는 것
     - 프로세서 스케출링 -> time out, priority changes
   - Block/Sleep
     - Running state -> asleep state
     - i/o 등 자원 할당 요청
     - 들어와야 할 input 이 없을 떄 asleep 상태에서 대기

![image-20200918103217829](.\images\image-20200918103217829.png)

4. Blocked/ Asleep state

   - asleep 상태에서 input이 들어왔다고 바로 running 상태로 가는 것은 아님
   - 다시 ready 상태로 돌아가 자원 할당을 기다려야 함 (wake-up)

   - 프로세서 외에 다른 자원을 기다리는 상태
     - 자원 할당은 system call에 의해 이루어 짐
   - Wake-up
     - Asleep state -> ready state

   ![image-20200918103534990](.\images\image-20200918103534990.png)

5. Suspended State

   - **메모리**를 할당 받지 못한(빼앗긴) 상태

   - suspended ready: created state에서 메모리를 할당받지 못해 ready가 못된 상태
   - suspended blocked: asleep 상태에서 메모리 마저 빼앗긴 상태

   - 메모리를 뺏길 때 memeory image를 swap device에 보관
     - 메모리의 현재 상태를 사진 형태로 보관 (memory image로 저장)
     - 나중에 다시 불렀을 때 이어가기 위해
     - swap device: 프로그램 정보 저장을 위한 특별한 파일 시스템
     - 다시 메모리 할당 받았을 떄 처음부터 하지말고 memory image의 상태부터
   - 커널 또는 사용자에 의해 발생

   - Swap-out(suspended), Swap-in(resume)
     - 메모리를 뺏기는 것 -> Swap-out
     - 메모리를 다시 할당 받는 것 -> Swap-in

![image-20200918104127679](.\images\image-20200918104127679.png)

6. Terminated/Zombie State
   - 프로세스 수행이 끝난 상태
   - 모든 자원 반납 후, 
   - 커널 내에 일부 PCB 정보만 남아 있는 상태
     - 이후 프로세스 관리를 위해 정보 수집

![image-20200918104842231](.\images\image-20200918104842231.png)

## Process State Diagram

- 위의 과정을 한 flow로 나타냄

![image-20200918104930666](.\images\image-20200918104930666.png)

- 프로세스 상태 및 특성

![image-20200918105019494](.\images\image-20200918105019494.png)

- 프로세스 관리를 위한 자료 구조

![image-20200918105212071](.\images\image-20200918105212071.png)