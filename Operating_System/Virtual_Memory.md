## Virtual Storage (Memory)

- Non-continuous allocation
- 사용자 프로그램을 여러 개의 block으로 분할
- 실행 시, 필요한 block들만 메모리에 적재
  - 나머지 block들은 swap device에 존재
- 기법들
  - Paging system
  - Segmentation system
  - Hybrid paging/segmentation system

### Address Mapping

- Continuous allocation

  - Relatvie address (상대 주소)
    - 프로그램의 시작 주소를 0으로 가정한 주소
  - Relocation (재배치)
    - 메모리 할당 후, 할당된 주소(allocation address)에 따라 상대 주소들을 조정하는 작업

  ![image-20201002163407393](images\image-20201002163407393.png)

- Non-continuous allocation

  - Virtual address (가상주소) = relative address
    - Logical address (논리 주소)
    - 연속된 메모리 할당을 가정한 주소
    - Non-continuous 이기 때문에 조각조각 나눠서 메모리에 올라갈 것이다.
  - Real address (실제주소) = absolute (physical)
    - 실제 메모리에 적재된 주소
  - Address mapping
    - Virtual address -> real address

  ![image-20201002163741143](images\image-20201002163741143.png)

  - **사용자/프로세스는 실행 프로그램 전체가 메모리에 연속적으로 적재되어있다고 가정하고 실행 할 수 있음**

### Block Mapping

- 사용자 프로그램을 block 단위로 분할/관리

  - 각 block에 대한 address mapping 정보 유지

- Virtual address : v = (b,d)

  - b = block number 원하는 데이터가 있는 block의 번호
  - d = displacement(offset) in a block 데이터가 block의 첫번째에서 얼마나 떨어져있는가

  ![image-20201002164049566](images\image-20201002164049566.png)

- Block map table (BMT)

  - Address mapping 정보 관리
    - kernel 공간에 프로세스마다 하나의 BMT를 가짐

  ![image-20201002164223835](images\image-20201002164223835.png)

  - Resident bit: 해당 블록이 메모리에 적재되었는지 여부(0 or 1)

  ![image-20201002164404050](images\image-20201002164404050.png)

  - block number b로 가서 실제로 메모리에 올라가 있는가 확인 (residence bit 가 1)
  - displacement 만큼 더해서 실제 데이터가 저장된 메모리 위치를 찾는다.

#### Block mapping sequence

1. 프로세스의 BMT에 접근
2. BMT에서 block b에 대한 항목을 찾음
3. Residence bit 검사
   1. Residence bit = 0 인 경우 (메모리에 올라가 있지 않다.)
      - swap device에서 해당 블록을 메모리로 가져 옴
      - BTM 업데이트 후 3-2 단계 수행
   2. Residence bit = 1 인 경우
      - BMT에서 b에 대한 real address 값 a 확인
4. 실제 주소 r 계산 (r = a + d)
5. r을 이용하여 메모리에 접근