### Software Components

- 가상 메모리 성능 향상을 위한 관리 기법들
  - Allocation strategies (할당 기법)
  - Fetch strategies
  - Placement strategies (배치 기법)
  - Replacement strategies (교체 기법)
  - Cleaning strategies (정리 기법)
  - Load control strategies (부하 조절 기법)

#### Allocation Strategies

- 각 프로세스에게 메모리를 얼마 만큼 줄 것인가?
  - Fixed allocation (고정 할당)	
    - 프로세스의 실행 동안 고정된 크기의 메모리 할당
  - Variable allocation (가변 할당)
    - 프로세스의 실행 동안 할당하는 메모리의 크기가 유동적
- 고려사항
  - 프로세스 실행에 필요한 메모리 양을 예측해야 함
  - 너무 큰 메모리 할당 (Too much allocation)
    - 메모리가 낭비 됨
  - 너무 적은 메모리 할당 (Too small allocation)
    - page fault rate 상승
    - 시스템 성능 저하  

#### Fetch Strategies

- 특정 page를 메모리에 **언제** 적재할 것인가?

  - Demand fetch (demand paging) -> 필요할때(프로세스가 요구할때) 적재
    - 프로세스가 참조하는 페이지들만 적재
    - page fault overhead (필요할때마다 가져오니까 page fault)

  - Anticipatory fetch (pre-paging)
    - 참조될 가능성이 높은 page 예측
    - 가까운 미래에 참조될 가능성이 높은 page를 미리 적재
    - 예측 성공 시, page fault overhead가 없음
    - prediction overhead (kernel의 개입), Hit ratio에 민감함

- 실제 대부분의 시스템은 **Demand fetch** 기법 사용

  - 일반적으로 준수한 성능을 보여 줌
  - Anticipatory fetch
    - prediction overhead, 잘못된 예측 시 자원 낭비 큼

#### Placement Strategies

- Page/Segment를 **어디에** 적재할 것인가?
- Paging system에는 불필요
- Segmentation system에서 배치 전략
  - 최적적합
  - 최악적합
  - 최초적합

#### Replacement Strategies

- 새로운 page를 **어떤 page와** 교체할 할것인가? (빈 page frame이 없는 경우)
-  To be continue...

#### Cleaning Strategies

- 변경된 page를 언제 write-back 할 것인가?
  - 변경된 내용을 swap-device에 반영
  - Demand cleaning
    - 해당 page에 메모리에서 내려올 때 write-back
  - Anticipatory cleaning (pre-cleaning)
    - 더 이상 변경될 가능성이 없다고 판단 할 때, 미리 write-back
    - page 교체 시 발생하는 write-back 시간 절약
    - write-back 이후, page 내용이 수정되면 overhead!
- 실제 대부분의 시스템은 Demand cleaning 기법 사용
  - 일반적으로 준수한 성능을 보여 줌
  - Anticipatory cleaning
    - prediction overhead, 잘못된 예측 시 자원 낭비가 큼

#### Load Control Strategies

- 시스템의 multi-programming degree 조절

  - multi-programming degree : 시스템에 들어온 프로세스의 수

  - Allocation strategies와 연계 됨

- 적정 수준의 multi-programming degree를 유지 해야 함

  - Plateau(고원) 영역으로 유지
  - 저부하 상태 (Under-loaded) -> 프로세스 수가 너무 낮음
    - 시스템 자원 낭비, 성능 저하
  - 고부하 상태 (Over-loaded) -> 프로세스 수가 너무 높음
    - 너무 많은 프로세스들이 메모리를 요청 -> page fault 과도하게 발생
    - 자원에 대한 경쟁 심화, 성능 저하
    - Thrashing(스레싱) 현상 발생
      - 과도한 page fault가 발생하는 현상

![image-20201006225917923](images\image-20201006225917923.png)