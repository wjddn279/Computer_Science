# OS supported SW solution

## Spinlock

- 정수형 변수 (S)
- 초기화, P(), V() 연산으로만 접근 가능
  - 위 연산들은 indivisible (or atomic) 연 -산
    - OS support -> P와 V연산은 실행 되는 도중 preemitive 되지 않고 끝까지 실행됨(도중에 쫓겨나지 않음)
    - 전체가 한 instruction cycle에 수행됨

![image-20200921235915771](.\images\image-20200921235915771.png)

- S : 물건의 갯수 (S == 1: 진입이 가능하다)
- P(S) 연산 : 물건을 꺼내가는 것 (진입)
  - 물건이 없다면(S == 0 : 진입 불가) 계속 기다리고 (while (S <= 0) do)
  - 있다면 S <- S - 1 하나 뺀다 (꺼내나감)
- V(S) 연산 : 물건을 집어 넣는 것
  - S <- S + 1 (물건 반납)
- 활용 예제

![image-20200922000254410](.\images\image-20200922000254410.png)

- active : spinlock 변수
- 초기상태 active는 1이고 Pi가 먼저 접근
- 맨 처음 Pi가 접근하면 P(active);에서 active를 0으로 만들고 CS에 진입
- Pj는 active가 0이기 때문에 진입 불가능 (OS support로 인해 Pi가 끝까지 진행 되는 것을 보장 받기 때문에(spinlock 특징) 상호 배제가 일어나지 않는다)
- Pi가 CS에서 나가면 active를 1로 만들고 Pj가 CS에 진입

- 문제점
  - 프로세스가 하나인 경우에는 사용 불가 (멀티 프로세서 시스템에서만 사용 가능)
  - Busy waiting (반복문을 탈출하지 못해 계속 loop를 돔)

## Semaphore

- 1965년 Dijkstra가 제안
- Busy waiting 문제 해결
- 음이 아닌 정수형 변수(S)
  - 초기화 연선, P(), V() 로만 접근 가능
  - P: Probern (검사)
  - V: Verhogen (증가)
- **임의의 S 변수 하나에 ready queue 하나가 할당 됨**
- Binary Semaphore
  - S가 0과 1 두 종류의 값만 갖는 경우
  - 상호배제나 프로세스 동기화의 목적으로 사용
- Counting Semaphore
  - S가 0이상의 정수값을 가질 수 있는 경우
  - Producer-Consumer 문제 등을 해결하기 위해 사용
    - 생산자-소비자 문제

- 초기화 연산
  - S 변수에 초기값을 부여하는 연산
- P() 연산, V() 연산

![image-20200922001600947](.\images\image-20200922001600947.png)

- P(S) 연산
  - Spinlock : S가 0 이라면 While 문을 계속 돌았음 (busy waiting)
  - Semaphore : S 가 0 이라면 Ready queue에 저장 (대기실에서 대기)
- V(S) 연산
  - 종료후 실행
  - Ready queue에 대기하는 프로세스가 있다면 그중 하나를 활성화

## Semaphore로 해결 가능한 동기화 문제들

1. Mutual Exclusion

![image-20200922001946420](.\images\image-20200922001946420.png)

- spinlock 대신 semaphore를 사용함으로서 busy waiting 문제 해결

2. Process Synchronization
   - Process들의 실행 순서 맞추기 (ex process A 가 실행 된 후 process B 가 실행되야 할 때 이 순서에 맞게 실행되도록 하는 것)
     - 프로세스들은 병행적이며, 비동기적으로 수행 (따라서 동기화로 순서를 임의로 맞춰줘야 함)

- 예제

![image-20200922002439430](.\images\image-20200922002439430.png)

![image-20200922002405251](.\images\image-20200922002405251.png)

- Pj가 먼저 와서 물건 S를 들고 있다고 생각 -> 그 뒤로 온 Pi는 대기실(ready queue)에서 대기
- Pj가 끝나면 S를 반납하고 ready queue에 있는 Pi를 깨우고 퇴장
- Pi가 물건 받고 입장

3. Producer-Consumer problem

- 생산자 프로세스
  - 메세지를 생성하는 프로세스 그룹
- 소비자 프로세스
  - 메세지를 전달받는 프로세스 그룹

![image-20200922002818365](.\images\image-20200922002818365.png)

- 생산자 프로세스가 생산 한 것을 놓고 있는 중에 소비자 프로세스가 가져가면 안됨
- 생산자 프로세스 끼리도 같은 곳에 동시에 두면 안됨
  - -> 동기화가 필요!!

- Producer-Consumer problem with single buffer
  - single-buffer : 공간이 1
  - 물건을 놓는 도중에는 들고가면 안되고, 물건을 놓고 있는데 또 놓으러 오면 안됨
  - -> Critical Section과 비슷한 개념 (한번에 한명만 접근 가능)

![image-20200922003202013](.\images\image-20200922003202013.png)

- 두개의 semaphore 변수 사용(consumed, produced)
- Consumed == 1 : buffer에 있는 메세지를 가져갔는가?(buffer가 비었는가? -> producer 접근) 
- Produced == 1: buffer에 메세지가 생성되었는가? (buffer에 뭔가 있는가? -> consumer 접근)
- 생산자: consumed == 1 이면 접근 가능 -> 접근 후 메세지 생산 -> produced = 1로 변경 후 종료(ready queue에 있다면 깨워줌)
- 소비자 : produced == 1 이면 접근 가능 (아니라면 ready queue에서 대기) -> 접근 후 메세지 소비 -> consumed = 1로 변경 후 종료

- Producer-Consumer problem with N-buffers

![image-20200922004210964](.\images\image-20200922004210964.png)

- mutexP, mutexC (상호 배제를 위한 semaphore) 즉, 현재 buffer 안에 P가 들어가 있다면 새로운 P는 들어갈 수 없고, C가 들어가 있다면 C가 들어갈 수 없다. -> WHY? 무언가 생산하거나 소비하고 있는데 다른녀석이 들어와서 만들거나 가져 갈 수 없다. 한번에 한명만 들어가서 일해라는 의미
- nrfull : 채워져 있는 buffer 수 (물건 수), nrempty : 비어있는 buffer 수 (새로만들 남은 공간의 수)

- Producer Pi:
  - P(nrempty);  만일 nrempty > 0 (새로만들 자리가 있다면 진입) 없다면 ready queue 대기
  - 중간과정 : 새로운 메세지를 만들어 버퍼에 저장
  - V(nrfull); 물건 수 증가
- Consumer Ci:
  - P(nrfull) : 만일 nrfull > 0 (가져갈 물건이 있다면 진입) 없다면 ready queue 대기
  - 중간 과정: 메세지를 가져감
  - V(nrempty); : 공간 수 증가

4. Reader-Writer problem

- Reader
  - 데이터에 대해 읽기 연산만 수행
- Writer
  - 데이터에 대해 갱신 연산을 수행
- 데이터 무결성 보장 필요
  - Reader들은 동시에 데이터 접근 가능
  - Writer들(reader와writer)이 동시에 데이터 접근 시, 상호배제(동기화) 필요
- 해결법
  - reader/writer 에 대한 우선권 부여

- Reader-writer problem (Reader preference solution) reader가 우선권 가지고 있음

![image-20200922005836279](.\images\image-20200922005836279.png)



## Semaphore

- No busy waiting
  - 기다려야 하는 프로세스는 block(asleep)상태가 됨
- Semaphore queue에 대한 wake-up순서는 비결정적 (랜덤으로 고름)
  - Stavation problem