### Memory Management in Paging system



#### Page Frame

- Page와 같은 크기로 미리 분할 하여 관리/사용

  - Page frame
  - FPM(Fixed Partition Multi programming) 기법과 유사

- Frame table

  - Page frame 당 하나의 entry
  - 구성
    - Allocated/available field
    - PID field
    - Link field: For free List (사용가능한 fp들을 연결) -> 빈 entry들에 대한 linked list를 만듬
    - AV : Free list header (free list의 시작점) -> 처음 비어있는 frame의 시작점

  ![image-20201002172723248](images\image-20201002172723248.png)

  - AV - > 현재 비어있는 page frame 중 첫번째의 page number
  - Link -> 현재 page frame 다음으로 비어있는 page frame을 연결
  - 만일 현재 AV가 가리키는 Page frame이 할당 되면 Link에 연결되어있는 다음의 비어있는 page frame을 AV가 가리키게 된다.

#### Page Sharing

- 여러 프로세스가 특정 page를 공유 가능
  - Non-continuous allocation
- 공유 가능 page
  - Procedure (function) pages
    - Pure code (reenter code)
  - Data page
    - Read only data
    - Read write data
      - 병행성(concurrency) 제어 기법 관리하에서만 가능

- 예제 - Editor(한글) 프로그램을 3명이 사용하는 경우

  ![image-20201002173420955](images\image-20201002173420955.png)

  - Editor 프로그램 pages : ed1 ~ ed3
  - 서로 공유해서 사용한다면 메모리에 올려놓고 같이 사용하면 됨
  - 3,4,6번에 page가 올라가 있고 각 프로세스의 페이지 테이블에 3,4,6번을 사용한다.
  - 쓸 데이터만 따로 구분하여 메모리에 따로 저장하여 읽고 쓴다 (data 1,2,3)

- Data page sharing

  ![image-20201002173705806](images\image-20201002173705806.png)

  - P1 과 P2 모두 같은 데이터의 Page Frame에 접근하면 동시에 데이터를 사용할 수 있다.

- Procedure Page Sharing

  - P1과 P2 각각 부르는 이름이 다르다면 실제로 찾는 위치자 달라질수 있다

  ![image-20201002174305815](images\image-20201002174305815.png)

  - 따라서 프로세스들이 Shared page에 대한 정보를 PMT의 같은 entry에 저장하도록 함

  ![image-20201002174159111](images\image-20201002174159111.png)

  

#### Page Protection

- 여러 프로세스가 page를 공유할때 -> 보안의 문제가 생길 수 있음

  - Protection bit 사용

  ![image-20201002174454216](images\image-20201002174454216.png)

  - 프로세스에 페이지 별 행동 권한을 부여함 으로서 보안을 보장할 수 있다
  -  ex) 페이지 0 ->1100 (메모리적재,읽기만 가능)

### Paging system - Summary

- 프로그램을 고정된 크기의 block으로 분할 -> page
- 메모리를 block size (page size) 로 미리 분할 -> page frame
  - 외부 단편화 문제 없음
  - 메모리 통합/ 압축 불필요
  - 프로그램의 논리적 구조 고려하지 않음
    - page sharing/protection이 복잡
- 필요한 page만 page frame에 적재하여 사용
  - 메모리의 효율적 활용
- Page mapping overhead
  - 메모리 공간 및 추가적인 메모리 접근이 필요
  - 전용 HW 활용으로 해결 가능
    - 하드웨어 비용 증가