### Hybrid paging/segmentation system

- Paging과 segmentation의 장점 결합
- 프로그램 분할
  1. 논리 단위의 segment로 분할
  2. 각 segment를 고정된 크기의 page들로 분할
- Page단위로 메모리에 적재

![image-20201004225736736](images\image-20201004225736736.png)

- segment로 나눈 후, 그 세그먼트를 page단위로 나눠서 메모리에 적재

#### Address mapping

- Virtual address : v = (s,p,d)
  - s : segment number
  - p : page number
  - d : offset in a page
- SMT와 PMT 모두 사용
  - 각 프로세스 마다 하나의 SMT
  - 각 Segmetn 마다 하나의 PMT
- Address mapping
  - Direct, associated 등
- 메모리 관리
  - FPM(Fixed partition multi-programming)

#### SMT in hybrid mechanism

![image-20201004230002702](images\image-20201004230002702.png)

- No residence bit -> 직접 메모리에 올라가는 단위가 아니기 때문

#### PMT for a segment k in hybrid mechanism

![image-20201004230101434](images\image-20201004230101434.png)

#### Address mapping tables

![image-20201004230158923](images\image-20201004230158923.png)

#### Direct (address) mapping

![image-20201004230328692](images\image-20201004230328692.png)

- 3번의 메모리 접근 -> overhead도 존재 (TLB 사용)

### Summary

- 논리적인 분할 (segment)와 고정 크기 분할(page)을 결합
  - page sharing/protection이 쉬움
  - 메모리 할당/관리 overhead가 작음
  - No external fragmentation
- 전체 테이블의 수 증가
  - 메모리 소모가 큼
  - Address mapping 과정이 복잡
- Direct mapping의 경우, 메모리 접근이 3배
  - 성능이 저하될 수 있음