# Main Memory Management

## 메모리의 종류

![image-20200928211343855](.\images\image-20200928211343855.png)

- 용어
  - Block
    - 보조기억장치(디스크)와 주기억장치(메모리) 사이의 데이터 전송 단위
    - Size: 1~4KB
    - 보조기억장치의 1bit만 읽어도 주기억장치에선 Block 만큼 올라옴
  - Word
    - 주기억장치와 레지스터 사이의 데이터 전송 단위
    - Size: 16~64bits

### Address Binding

- 프로그램의 논리 주소를 실제 메모리의 물리 주소로 매핑(mapping)하는 작업
  - ex) 내가 int a 를 선언했다, a는 메모리에 100이라는 물리적 주소에 저장된다
  -  -> a 라는 논리 주소를 실제 저장된 물리 주소 100에 매핑
- Binding 시점에 따른 구분
  - Compile time binding -> 컴파일 할때 주소 바인딩
  - Load time binding -> 프로그램을 메모리에 올릴때 주소 바인딩
  - Run time binding -> 실행할때 주소 바인딩

![image-20200928212522871](.\images\image-20200928212522871.png)

1. Compile time binding

   - 프로세스가 메모리에 적재될 위치를 컴파일러가 알수 있는 경우
     - 위치가 변하지 않음
   - 프로그램 전체가 메모리에 올라가야 함

2. Load time binding

   - 메모리 적재 위치를 컴파일 시점에서 모르면, 대체 가능한 상대 주소를 생성
     - 상대 주소: 기준점을 변수 (u)로 잡고 u+100, u+200으로 주소를 설정
   - 적재 시점(load time)에 시작 주소를 반영하여 사용자 코드 상의 주소를 재설정
   - 프로그램 전체가 메모리에 올라가야 함

   ![image-20200928212904980](.\images\image-20200928212904980.png)

- 처음 실행시에는 시작 주소를 0으로 가정 (상대적인 주소로 설정)
  - ex) 0 , 0 +240 , 0 + 800
- loading 후 메모리에 올라갔을 때 시작주소가 400으로 바뀜
- 나머지도 400을 기준으로 바뀌고 target또한 바뀐다.

3. Run-time binding
   - Address binding 을 수행시간 까지 연기
     - 프로세스가 수행 도중 다른 메모리 위치로 이동할 수 있음
   - HW의 도움이 필요
     - MMU: Memory Management Unit
   - 대부분의 OS가 사용

## Dynamic Loading

- 모든 루틴(function)을 교체 가능한 형태로 디스크에 저장
- 실제 호출 전 까지는 루틴을 적재하지 않음
  - 메인 프로그램만 메모리에 적재하여 수행
  - 루틴의 호출 시점에 address binding 수행
- 장점
  - 메모리 공간의 효율적 사용

## Swapping

- 프로세서 할당이 끝나고 수행 완료 된 프로세스는 swap-device로 보내고 (Swap-out)
- 새롭게 시작하는 프로세스는 메모리에 적재(Swap-in)