# 교착 상태 (Deadlock Resolution)

## Deadlock의 개념

- Blocked / Asleep state
  - 프로세스가 특정 이벤트를 기다리는 상태
  - 프로세스가 필요한 자원을 기다리는 상태

- Deadlock state
  - 프로세스가 **발생 가능성이 없는 이벤트**를 기다리는 경우
    - 프로세스가 deadlock 상태에 있음 (asleep 상태 중 하나)
  - 시스템 내에 deadlock에 빠진 프로세스가 있는 경우
    - 시스템이 deadlock 상태에 있음

- Deadlock vs Starvation
  - Deadlock : asleep 상태에서 자원을 기다리나 올 가능성이 없는 상태
  - Starvation : ready 상태에서 자원(CPU) 할당을 기다리나 오지 않는 상태(울수는 있으나 오지 않는 상태)

![image-20200928095717135](images\image-20200928095717135.png)

## 자원의 분류

- 일반적 분류
  - Hardware resource vs Software resources
- 다른 분류법
  - 선점 가능 여부에 따른 분류
  - 할당 단위에 따른 분류
  - 동시 사용 가능 여부에 따른 분류
  - 재사용 가능 여부에 따른 분류

1. 선점 가능 여부에 따른 분류
   - Preemptible resources
     - 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원
     - 프로세스가 그 자원을 사용하다가 뺏겨도 다시 받으면 그대로 사용 가능
     - Processor (context switching), memory (swap-device) 등
   - Non-preemptible resources
     - 선점 당하면, 이후 진행에 문제가 발생하는 자우너
       - Rollback, restart등 특별한 동작이 필요
     - E.g, disk drive등 (usb에 파일을 복사하고 있는데 usb를 뽑으면 진행이 되지 않음)
2. 할당 단위에 따른 분류
   - Total allocation resources 
     - 자원 전체를 프로세스에게 할당 (한번에 하나의 프로세스만 점유 가능)
     - Processor, disk drive 등
   - Partitioned allocation resources
     - 하나의 자원을 여러 조각으로 나누어, 여러 프로세스들에게 할당
     - memory
3. 동시 사용 가능 여부에 따른 분류
   - Exclusive allocation resources
     - 한 순간에 한 프로세스만 사용 가능한 자원
     - 프로세스 마다 할당된 자원 공간이 있고 다른 프로세스가 그것을 쓸 수 없다
     - Processor, memory(나눠진 공간을 각기 다른 프로세스가 사용), disk drive 등
   - Shared allocation resource
     - 여러 프로세스가 동시에 사용 가능한 자원
     - Program(sw), shared data 등
4. 재사용 가능 여부에 따른 분류
   - SR (Serailly-reusable Resources)
     - 시스템 내에 항상 존재하는 자원
     - 사용이 끝나면, 다른 프로세스가 사용 가능
     - Processor, memory, disk drive, program 등
   - CR (Consumable Resources)
     - 한 프로세스가 사용한 후에 사라지는 자원
     - signal, message 등

- **Deadlock을 발생 시킬 수 있는 자원의 형태**

  - Non-preemptible resources
    - 한번 할당 받으면 끝까지 하는데 반납하지 않으면 쓸 수 없다.
  - Exclusive allocation resources
    - 혼자 쓰기 때문에 문제가 생길 수 있다.
  - Serially reusable resources
    - 할당 단위는 영향을 미치지 않는다!!

- CR을 대상으로 하는 Deadlock model

  - 너무 복잡하다!! (고려하지 않음)

  