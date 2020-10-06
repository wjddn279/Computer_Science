### Virtual Memory Management

- 가상 메모리 (기억장치)
  - Non-continous allocation
    - 사용자 프로그램을 block으로 분할하여 적재/실행
  - Paging/Segmentation system
- 가상 메모리 관리의 목적
  - 가상 메모리 시스템 성능 최적화
    - Cost model -> 성능의 지표
    - 다양한 최적화 기법

### Cost Model for Virtual Memory system

- Page fault frequency (발생 빈도)
- Page fault rate (발생률)
- Page fault rate를 최소화 할 수 있도록 전략들을 설계해야 함
  - Context switch 및 kernel 개입을 최소화
  - 시스템 성능 향상
-  Page reference string (d) -> 페이지 참조 문자열
  - 프로세스의 수행 중 참조한 페이지 번호 순서
  - w = r1r2...rk...rt (기준 및 평가)
    - ri = 페이지 번호
    - N : 프로세스의 페이지 수 (0 ~ N-1)

- Page fault rate = F(w)
  - F(w) = (# of page faults during w) / |w|

#### Hardware Components

- Address translation device (주소 사상 장치)

  - 주소 사상을 효율적으로 수행하기 위해 사용
    - TLB (associated memories), Dedicated page-table register, Cache memories

- Bit Vectors

  - Page 사용 상황에 대한 정보를 기록하는 비트들
  - Reference bits (used bit)
    - 참조 비트
  - Update bits (modified bits, write bits, **dirty bits**)
    - 갱신 비트

  ![image-20201006223110385](images\image-20201006223110385.png)

  - Bit vector들은 PMT에 저장

- Reference bit vector

  - 메모리에 적재된 각각의 page가 최근에 참조 되었는지를 표시

  - 운영

    1. 프로세스에 의해 참조되면 해당 page의 ref.bit를 1로 설정

    2. **주기적으로** 모든 reference bit를  0으로 초기화

  - Reference bit를 확인함으로서 최근에 참조된 page들을 확인 가능
    - Locality

  ![image-20201006223313435](images\image-20201006223313435.png)

- Update bit vector
  - Page가 메모리에 적재된 후, 프로세스에 의해 수정 되었는지를 표시
  - 주기적 초기화 없음
  - page가 메모리에 적재되고 수정되었다면 swap device의 내용과 달라짐
  - 바뀌었다면 swap device에 다시 쓰여야 하고 그를 체크하기 위해 update bit vector를 사용
  - Update bit = 1
    - 해당 page의 (Main memory 상 내용) != (Swap device의 내용)
    - 해당 page에 대한 write-back (to swap device)이 필요