## Continous Memory Allocation

- 프로세스 (context)를 하나의 연속된 메모리 공간에 할당하는 정책
  - 프로세스 하나를 나누지 않고 통쨰로 연속적으로 메모리에 올림
  - 프로그램, 데이터, 스택 등
- 메모리 구성 정책
  - 메모리에 동시에 올라갈 수 있는 프로세스 수
    - 시스템에 메모리가 몇개 올라 갈 수 있는가
    - Multiprogramming degree
  - 각 프로세스에게 할당되는 메모리 공간 크기
  - 메모리 분할 방법

### Uni-programming

- Multiprogramming degree = 1
  - 프로세스가 한번에 하나만 메모리에 올라간다.

- 하나의 프로세스만 메모리 상에 존재
- 가장 간단한 메모리 관리 기법

- 문제점
  - 프로그램의 크기 > 메모리의 크기
- 해결법
  - overlay structure
    - 메모리에 현재 필요한 영역만 적재
    - 커널이 해줄 수 없고 사용자가 직접 처리해줘야 한다.
    - 사용자가 프로그램의 흐름 및 자료구조를 모두 알고 있어야 함

- 문제점
  - 커널(kernel 보호)
- 해결방법
  - 경계 레지스터(boundary register) 사용
  - 경계 레지스터: 경계값의 주소를 저장하는 레지스터(그로 인해 영역 침범 방지)
- 문제점  -> 하나만 올라간다. 충분히 공간이 남는데도 사용을 안함
  - Low system resource utilization
  - Low system performance
- 해결법
  - Multi-programming

### Multi-programming

1. fixed partition multi-programming
2. variable partition multi-programming

#### fixed partition multi-programming

- 메모리 공간을 고정된 크기로 분할
  - 미리 분할되어 있음
- 각 프로세스는 하나의 partition(분할)에 적재
  - 프로세스 각각이 파티션으로 나눠져 있음
  - Process : Partition = 1 : 1

- Partition의 수 = K
  - Multiprogramming degree = K
- 자료구조의 예

![image-20200928214831266](images\image-20200928214831266.png)

- 커널 및 사용자 영역 보호 
  - 경계 레지스터를 통해 영역 침범 방지

#### Fragmentation (단편화)

- Internal fragmentation
  - 내부 단편화
  - Partition 크기 > Process 크기
    - 프로세스가 들어갈 수는 있는데 낭비가 생김
    - 메모리가 낭비 됨
- External fragmentation
  - 외부 단편화
  - (남은 메모리 크기 > Process 크기) 지만 연속된 공간이 아님
  - 남은 메모리 공간은 충분한데, partition으로 나눠져 있어(연속된 공간이 아니여서) 들어가지못해 메모리가 낭비

#### Fixed partition multi-programming Summary

- 고정된 크기로 메모리 미리 분할
- 메모리 관리가 간편함
- 시스템 자원이 낭비 될 수 있음
- 내부/ 외부 단편화 발생



### Variable Partition Multiprogramming (VPM)

- 초기에는 전체가 하나의 영역
- 프로세스를 처리하는 과정에서 메모리 공간이 동적으로 분할
- No internal fragmentation (내부 단편화 x, 필요한 만큼 파티션이 분할되니 낭비 x)

##### VPM example

![image-20200928215732721](images\image-20200928215732721.png)

![image-20200928215806826](images\image-20200928215806826.png)

![image-20200928215831382](images\image-20200928215831382.png)

![image-20200928215909798](images\image-20200928215909798.png)

- 상대 주소 u를 기준으로 할당
- 내부 단편화 발생하지 않음

![image-20200928215942894](images\image-20200928215942894.png)

- B가 나가버림 (가운데 10MB의 공백 파티션 발생)

![image-20200928220017866](images\image-20200928220017866.png)

- E가 들어옴 (들어갈 수 있는 밑의 위치에 들어감)

![image-20200928220100180](images\image-20200928220100180.png)

- D가 나감 (가운데 빈공간이 또 발생)

![image-20200928220146154](images\image-20200928220146154.png)

- 어디에  P (5MB)가 들어가야 적합할까?

#### 배치 전략 (Placement strategies)

- First-fit(최초 적합)

  - 충분한 크기를 가진 첫 번째 partition을 선택
  - Simple and low overhead
  - 공간 활용률이 떨어질 수 있음

- Best-fit(최적 적합)

  - process가 들어갈 수 있는 partition 중 가장 작은 곳 선택
  - 탐색시간이 오래걸림
    - 모든 partiton을 살펴봐야 함

  - 크기가 큰 partition을 유지 할 수 있음
  - 작은 크기의 partition이 많이 발생
    - ex) 15KB의 공백에 14KB가 들어감 -> 1KB의 공백밖에 남지 않음, 사용하기 힘듬
    - 활용하기 너무 작은 크기

- Worst-fit (최악 적합)

  - Process가 들어갈 수 있는 partition중 가장 큰 곳 선택
  - 탐색시간이 오래걸림
    - 모든 partition을 살펴봐야 함
  - 작은 크기의 partition 발생을 줄일 수 있음
    - ex) partition내부에도 공간이 많이 남기 때문에 사용할 가능성 높음
  - 큰 크기의 partition 확보가 어려움
    - 큰 프로세스에게 필요한 크기

- Next-fit (순차 최초 적합)
  - 최초 적합 전략과 유사
  - State table에서 마지막으로 탐색한 위치부터 탐색
    - 처음부터 탐색하는 것이 아닌 마지막에 할당한 메모리 위치부터 탐색 시작
    - 그 후 적합한 메모리에 할당
  - 메모리 영역의 사용 빈도 균등화
  - Low overhead



- 어디에 배치해야 할 것인가?

![image-20200928222129617](images\image-20200928222129617.png)

- 외부 단편화 문제

### Coalescing holes (공간 통합)

- 인접한 빈 영역을 하나의 partition으로 통합
  - process가 memory를 release하고 나가면 수행
  - Low overhead

![image-20200928222357474](images\image-20200928222357474.png)

- (a) -> (b) A가 나감
- 제일 위의 20MB, 10MB의 공간을 합쳐서 하나의 공간으로 만듬 (Variable parition이라 가능)
- 공간이 생길때마다 하나로 합쳐줘서 원하는 크기의 partition을 만듬

### Storage Compaction (메모리 압축)

- Example

![image-20200928222546536](images\image-20200928222546536.png)

- 빈공간을 한번에 하나로 합침

- 모든 빈 공간을 하나로 통합
- 프로세스 처리에 필요한 적재 공간 확보가 필요 할 때 수행
- High overhead
  - 모든 process 재배치 (process 중지)
  - 많은 시스템 자원을 소비

### Continuous Memory Allocation Summary

- Uni-programming
  - simple
  - Fragmentation problem
- Fixed partition multi-programming (FPM)
- Variable partition multi-programming (VPM)
  - Placement strategies
    - first-fit, Best-fit, Worst-fit, Next-fit
  - External fragmentation issue
    - Coalescing holes (공간 통합)
    - Storage compaction (메모리 합축)