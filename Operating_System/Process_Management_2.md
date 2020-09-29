## 인터럽트 (Interrupt)

- 예상치 못한, 외부에서 발생한 이벤트
- 인터럽트의 종류
  - I/O interrupt (ex. 게임 할떄의 마우스/키보드 클릭)
  - Clock interrupt
  - Console interrupt(콘솔 창 입력)
  - Program check interrupt
  - Machine check interrupt
  - Inter-process interrupt
  - system call interrupt

## 인터럽트 처리 과정

- 공부를 하는데 친구가 건들면?  -> 반응을 하고 (입력에 대한 응답 후) 다시 하던 거 함
- 인터럽트 발생 -> 프로세스 중단 -> 인터럽트 처리
- Interrupt handling -> interrupt의 원인과 발생 장소 파악, 인터럽트 서비스할건지
- Interrupt service -> 인터럽트 처리를 위한 서비스

![image-20200918105734690](images\image-20200918105734690.png)

1. 프로세스 Pi가 프로세스를 수행중 interrupt 발생

2. 커널이 개입하여 프로세스 중단 시킴
3. 중단된 프로세스는 커널에 의해 Context saving 됨
   - Context saving: 중단된 프로세스의 상태를 PCB에 저장 (책갈피의 개념)

4. Interrupt handling
5. Interrupt service
6. PCB에 있는 프로세스 다시 불러옴(Pi가 아닌 다른 context인 Pj가 들어 올 수 있다.)

![image-20200918110451149](images\image-20200918110451149.png)

## Context Switching (문맥 교환)

- Context
  - 프로세스와 관련된 정보들의 집함
    - CPU register context -> in CPU
    - Code & data, Stack, PCB -> in memory
- Context Saving
  - 현재 프로세스의 Register context를 저장하는 작업
- Context restoring
  - Register context를 프로세스로 복구하는 작업
- Context switching (saving + restoring)
  - 실행 중인 프로세스의 context를 저장하고, 앞으로 실행 할 프로세스의 context를 복구
  - 커널의 개입으로 이루어짐

## Context Switch Overhead

- Context switching에 소요되는 비용
  - OS마다 다름
  - OS의 성능에 큰 영향을 줌
- 불필요한 Context switching을 줄이는 것이 중요
  - ex) 스레드(thread 사용) 등