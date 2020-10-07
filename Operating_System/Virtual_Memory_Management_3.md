### Locality

- 프로세스가 프로그램/데이터의 특정 영역을 집중적으로 참조하는 현상
- 원인
  - Loop structure in program
  - Array, structure 등의 데이터 구조
- 공간적 지역성 (Spatial locality)
  - 참조한 영역과 인접한 영역을 참조하는 특성
- 시간적 지역성 (Temporal locality)
  - 한 번 참조한 영역을 곧 다시 참조하는 특성

### Locality (Example)

- 가정

  - Paging system
  - Page size = 1000 words
  - Machine instruction size = 1 word
  - 주소 지정은 word 단위로 이루어짐
  - 프로그램은 4번 page에 continous allocation 됨
  - n = 1000

  ```pseudocode
  # 4번 페이지에 적재 되어 있음
  for <- 1 to n do
  	A[i] <- B[i] + C[i]
  endfor
  ```

  ![image-20201007192233571](images\image-20201007192233571.png)

  - w (refernce string) = 494944 (474846444)^1000번 반복
  - 9000번의 메모리 참조 중 5개의 page만을 집중적으로 접근하게 됨 -> locality

### Replacement Strategies

- Fixed allocation -> 정해진 수의 page frame을 프로세서에 할당
  - MIN(OPT, B0) algorithm
  - Random algorithm
  - FIFO(First In First Out) algorithm
  - LRU(Least Recently Used) algorithm
  - LFU(Least Frequently Used) algorithm
  - NUR(Not Used Recently) algorithm
  - Clock algorithm
  - Second chance algorithm
- Variable allocation
  - WS(Working Set) algorithm
  - PFF(Page Fault Frequency) algorithm
  - VMIN(Variable MIN) algorithm

#### Min Algorithm (OPT algorithm)

- 1966년 Belady에 의해 제시
- Minimize page fault frequency (proved)
  - optimal solution
- 기법
  - 앞으로 가장 오랫동안 참조되지 않을 page 교체
    - tie-breaking rule: page 번호가 가장 큰/작은 페이지 교체
- 실현 불가능한 기법 
  - Page reference string을 미리 알고 있어야 함
  - 앞으로 어떤 페이지를 읽을지 알 수 없기 때문에 불가능
- 교체 기법의 성능 평가 도구(기준)로 사용 됨

![image-20201007193650621](images\image-20201007193650621.png)

#### Random Algorithm

- 무작위로 교체할 page 선택
- Low overhead
- No policy

#### FIFO algorithm

- First In First Out

  - 가장 오래된 page를 교체

- Page가 적재 된 시간을 기억하고 있어야 함

- 자주 사용되는 page가 교체 될 가능성이 높음

  - Locality에 대한 고려가 없음

- FIFO anomaly (Belady's anomaly)

  - FIFO 알고리즘의 경우, 더 많은 page frame을 할당 받음에도 불구하고 page fault의 수가 증가하는 경우가 있음

  ![image-20201007194621040](images\image-20201007194621040.png)

  - 메모리 할당을 늘렸지만 page faults는 더 늘어남

#### LRU (Least Recently Used) Algorothm

- 가장 오랫동안 참조되지 않은 page를 교체
- page 참조 시 마다 시간을 기록해야 함
- Locality에 기반을 둔 교체 기법
- MIN algorithm에 근접한 성능을 보여줌
- 실제로 가장 많이 활용되는 기법

![image-20201007194903688](images\image-20201007194903688.png)

- 단점
  - 참조 시 마다 시간을 기록해야 함 (Overhead)
    - 간소화된 정보 수집으로 해소 가능
      - 예) 정확한 시간 대신, 순서만 기록
  - Loop 실행에 필요한 크기보다 작은 수의 page frame이 할당 된 경우, page fault 수가 급격히 증가함
    - 예) loop를 위한 |Ref.string| = 4 / 할당된 page frame이 3개
    - 4개의 loop를 돌떄 page가 3개면 loop를 돌때마다 page fault가 발생한다.
    - Allocation 기법에서 해결 해야 함 (3개 할당된 것을 4개로 늘려줌)

#### LFU (Least Frequently Used) Algorithm

- 가장 참조 횟수가 적은 Page를 교체

  - Tie-breaking rule : LRU

- Page 참조 시 마다, 참조 횟수를 누적 시켜야 함

- Locality 활용

  - LRU 대비 적은 overhead

- 단점

  - 최근 적재된 참조될 가능성인 높은 page가 교체 될 가능성이 있음
  - 참조 횟수 누적 overhead

   ![image-20201007201025516](images\image-20201007201025516.png)

- Example

  - 4 page frames for the process, initially empty
  - w = 1 2 6 1 4 5 1 2 1 4 5 6 4 5

   ![image-20201007201207365](images\image-20201007201207365.png)

#### NUR (Not Used Recently) Algorithm

- LRU approximation scheme

  - LRU보다 적은 overhead로 비슷한 성능 달성 목적

- Bit vector 사용

  - Reference bit vector (r) , Update bit vector (m)

- 교체 순서

  1. (r,m) = (0,0)
  2. (r,m) = (0,1)
  3. (r,m) = (1,0)
  4. (r,m) = (1,1)

  - r 이 0인게 우선 -> 최근에 참조 되지 않은 것을 우선적으로 out
  - 그 다음 m이 0인게 우선 -> write back이 안일어나는 것을  그 다음

![image-20201007201543140](images\image-20201007201543140.png)

#### Clock Algorithm

- IBM VM/370 OS
- Reference bit 사용함
  - 주기적인 초기화 없음
- Page frame 들을 순차적으로 가리키는 pointer(시계바늘)을 사용하여 교체될 page 결정
- Pointer를 돌리면서 교체 page 결정
  - 현재 가리키고 있는 page의 reference bit(r) 확인
  - r = 0 인 경우, 교체 page로 결정
  - r = 1 인 경우, reference bit 초기화 후 pointer 이동
- 먼저 적재된 page가 교체될 가능성이 높음
  - FIFO와 유사
- Reference bit를 사용하여 교체 페이지 결정
  - LRU (or NUR) 과 유사 -> Locality

#### Second Chance Algorithm

- Clock algorithm과 유사
- Update bit (m) 도 함께 고려 함
  - 현재 가리키고 있는 page의 (r,m) 확인
  - (0,0) : 교체 page로 결정
  - (0,1) : -> (0,0), write back (cleaning) list에 추가 후 이동
  - (1,0) : -> (0,0) 후 이동
  - (1,1) : -> (0,1) 후 이동33