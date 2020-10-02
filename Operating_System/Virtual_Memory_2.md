### Paging system

- 프로그램을 같은 크기의 블록으로 분할 (pages)

- Terminologies

  - page 
    - 프로그램(프로세스)의 분할된 block
  - page frame 
    - 페이지를 넣는 메모리의 틀
    - 메모리의 분할 영역
    - Page와 같은 크기로 분할

  ![image-20201002165227847](images\image-20201002165227847.png)

  - swap device -> 가상 메모리와 같은 개념
  - Non-continuous system 이므로 memory에 불연속적으로 적재

- 특징

  - 논리적 분할이 아님(크기에 따른 분할)
    - page 공유(sharing) 및 보호(protection) 과정이 복잡함
      - segmentation 대비
    - simple and efficient
      - segmentation 대비
    - No external fragmentation (외부 단편화 x)
      - Internal fragmentation 발생 가능 -> 10을 3의 페이지 크기로 나누면 남는 1은 사용할 수 없다 -> 내부 단편화

### Paging mapping

- block mapping 과 거의 유사

- Virtual address : v = (p,d)
  - p : page number
  - d : displacement(offset)
- Address mapping
  - PMT(Page Map Table) 사용
- Address mapping mechanism
  - Direct mapping (직접 사상)  -> block mapping과 거의 유사
  - Associativ mapping (연관 사상)
    - TLB(Translation Look-aside Buffer)
  - Hybrid direct/associative mapping

- Page Map Table (PMT)

  ![image-20201002170213309](images\image-20201002170213309.png)

#### Direct mapping

- Block mapping 방법과 유사
- 가정
  - PMT를 커널 안에 저장
  - PMT entry size = entrySize (페이지 전체 갯수)
  - Page size = pageSize (페이지 하나의 크기)

![image-20201002170456731](images\image-20201002170456731.png)

1. 해당 프로세스의 PMT가 저장되어있는 주소 b에 접근
2. 해당 PMT에서 page p에대한 entry 찾음
   - p의 entry 위치 = b + p * entrySize
3. 찾아진 entry의 존재 비트 검사
   1.  Residence bit = 0 인 경우 (page fault)
      - swap device에서 해당 page를 메모리로 적재 -> context switching 발생 (overhead)
      - PMT를 갱신 후, 3-2 단계 수행
   2. Residence bit = 1 인 경우
      - 해당 entry에서 page frame 번호 p'를 확인

4. p'와 가상 주소의 변위 d를 사용하여 실제 주소 r 형성
   - r = p' * pageSize + d
5. 실제 주소 r로 주기억장치에 접근

- 문제점
  - 메모리 접근 횟수가 2배 (한번 mapping 하기 위해 PMT에 접근하기 위해 1번, 실제 주소를 찾기 위해 1번 총 2번 접근해야 함)
  - PMT를 위한 메모리 공간 필요
- 해결방안
  - Associative mapping (TLB) -> 메모리 접근횟수에 따른 성능 저하 해결
  - PMT를 위한 전용 기억장치(공간) 사용
    - Dedicated register or cache memory
  - Hierarchical paging
  - Hashed page table
  - Inverted page table

### Associative mapping

- TLB(Translation Look-aside Buffer)에 PMT 적재
  - Associative high speed memory
- PMT를 통한 병렬 탐색
- Low overhead, high speed

![image-20201002171401323](images\image-20201002171401323.png)

- TLB : parallel search가 가능한 hardware
  - 비싼 하드웨어 -> 큰 PMT를 다루기가 어려움

### Hybrid directive/Associative mapping

- 두 기법을 혼합하여 사용
  - HW 비용은 줄이고, Associative mapping의 장점 활용
- 작은 크기의 TLB 사용
  - PMT: 메모리(커널 공간)에 저장
  - TLB: PMT 중 일부 entry들을 적재
    - 최근에 사용된 page들에 대한 entry 저장
  - Locality (지역성) 활용
    - 프로그램의 수행과정에서 한번 접근한 영역을 다시 접근 또는 인접 영역을 접근 할 가능성이 높음

- 프로세스의 PMT가 TLB에 적재되어있는지 확인
  1. TLB에 적재되어있다?
     1. Residence bit를 검사하고 page frame 번호 확인
  2. 없다?
     1. Direct mapping으로 page frame 번호 확인
     2. 해당 PMT entry를 TLB에 적재함

![image-20201002171933871](images\image-20201002171933871.png)