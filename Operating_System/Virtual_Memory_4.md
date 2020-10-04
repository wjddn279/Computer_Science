### Segmentation System

- 프로그램을 논리적 block으로 분할 (segment)

  - Block의 크기가 서로 다를 수 있음
    - 예) stack, heap, main procedure, shared lib 
  - Paging system : block의 크기가 서로 똑같음 (segmentation과 비슷)

- 특징

  - 메모리를 미리 분할 하지 않음
    - VPM(Variant Partition Multi system) 과 유사
  - Segment sharing/protection이 용이 함
  - Address mapping 및 메모리 관리의 overhead가 큼
  - No internal fragmentation(내부 단편화 x)
    - 필요한 만큼 짤라서 쓰기 때문에 내부 단편화는 발생 x
    - External fragmentation 발생 

  ![image-20201004215204061](images\image-20201004215204061.png)
  - Swap device -> virtual memory 안에 존재
  - segment들이 논리적으로 분할

 #### Address mapping

- Virtual address : v = (s,d)
  - s : segment number
  - d : displacement in a segment
- Segment Map Table (SMT)
- Address mapping mechanism
  - paging system과 유사

#### Segment map table

![image-20201004215523638](images\image-20201004215523638.png)

- secondary storage address : swap device에 저장된 위치
- segment address in memory: 메모리에 올라갔다면 어디에 올라갔는가

- segment length : segment의 크기를 기록
- protection bits(R/W/X/A) : 프로세스가 가지는 권한 명시

#### Direct mapping

![image-20201004220015464](images\image-20201004220015464.png)

1. 프로세스의 SMT가 저장되어 있는 주소 b에 접근

2. SMT에서 segment s의 entry 찾음

   - s의 entry 위치 = b + s * entrySize

3. 찾아진 Entry에 대해 다음 단계들을 순차적으로 실행

   1. 존재 비트가 0 인 경우 (residence bit = 0)

      - missing **segment fault**
      - swap device로 부터 해당 segment를 메모리로 적재
      - SMT를 갱신

   2. 변위(d)가 segment 길이보다 큰 경우 (d>ls)

      - segment의 범위를 넘은 곳을 접근하려고 하는 것

      - segment overflow exception 처리 모듈을 호출

   3. 허가되지 않은 연산일 경우 (protection bit field 검사)

      - segment protection exception 처리 모듈을 호출

4. 실제 주소 r 계산 (r = as + d)

5. r로 메모리에 접근

#### Memory management

- VPM(Varible partition multi programming)과 유사

  - segment 적재 시, 크기에 맞추어 분할 후 적재

  ![image-20201004220838297](images\image-20201004220838297.png)

#### Segment sharing/protection

- 논리적으로 분할되어 있어, 공유 및 보호가 용이함

![image-20201004221024661](images\image-20201004221024661.png)

### Segmentation System - Summary

- 프로그램을 논리 단위로 분할 (Segment) / 메모리를 동적으로 분할
  - 내부 단편화 문제 없음
  - Segment sharing/protection이 용이함
  - Paging system 대비 관리 overhead가 큼
- 필요한 segment만 메모리에 적재하여 사용
  - 메모리의 효율적 활용
- Segment mapping overhead
  - 메모리 공간 및 추가적인 메모리 접근이 필요
  - 전용 HW활용으로 해결 가능

#### Paging vs Segmentation

- Pagigng system
  - simple
  - low overhead
  - No logical concept for partitioning
  - complex page sharing mechanism
- Segmentation System
  - high management overhead
  - logical concept for partitioning
  - simple and easy sharing mechanism