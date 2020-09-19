## Multi Level Queue (MLQ)

- 기존 스케줄링은 하나의 ready queue에서 모든 스케줄링 처리
- 작업 (or 우선순위) 별 별도의 ready queue를 가짐 (Ready queue를 여러개 사용)
  - 최조 배정 된 queue를 벗어나지 못함
  - 각각의 queue는 자신만의 스케줄링 기법 사용
- Queue 사이에는 우선순위 기반의 스케줄링 사용
  - fixed-priority, preemptive scheduling
- 장점
  - 빠른 응답시간 (?)
- 단점
  - 여러 개의 queue 관리 등 스케줄링 overhead-
  - 우선 순위가 낮은 queue는 starvation 현상 발생 (급해도 올라가지 못함)

![image-20200919132502967](.\images\image-20200919132502967.png)

## Multi Level Feedback Queue (MFQ)

- 프로세스의 Queue간 이동이 허용된 MLQ
- MLQ는 queue간의 이동이 불가능해 낮은 우선순위의 queue의 process가 빠르게 처리 되야 할때도 그러지 못하지만 MFQ는 상황에 따라 올라 갈 수 있다.
- Feedback을 통해 우선 순위 조정
  - 현재까지의 프로세서 사용 정보(패턴) 활용
- 특성
  - Dynamic prioruty
  - preemptive scheduling
  - Favor short burst time proccess
  - Favor I/O bounded processes
  - Improve adaptability
- 프로세스에 대한 사전 정보 없이 SPN, SRTN, HRRN(BT를 예상해야 했음)기법의 효과를 볼 수 있음

- 단점
  - 설계 및 구현이 복잡, 스케줄링 overhead 가 큼
  - startvation 문제 
- 변형
  - 각 준비 큐 마다 시간 할당량을 다르게 배정
    - 프로세스의 특성에 맞는 형태로 시스템 운영 가능
  - 입출력 위주 프로세스(I/O bounded process) 들을 상위 단계의 큐로 이동, 우선 순위 높임
    - 프로세스가 block 될 때 상위의 준비 큐로 진입하게 함
    - 시스템 전체의 평균 응답 시간 줄임, 입출력 작업 분산 시킴
    - I/O bounded는 cpu를 잠깐만 사용하기 때문에 우선 순위 올리면 처리량 상승 가능
    - 반대로 compute bounded 는 아래로 내림
  - 대시 시간이 지정된 시간을 초과한 프로세스들을 상위 큐로 이동
    - 에이징 기법 (starvation 방지)

- Parameters for MFQ scheduling
  - queue의 수
  - queue별 스케줄링 알고리즘
  - 우선 순위 조정 기준
  - 최초 queue 배정 방법