## 운영체제의 역할

1. 편리성 (User interface) -> CUI, GUI, EUCI

2. 효율성 (Resource management) -> HW resource, SW resource

3. 프로세스 및 스레드 관리

- 프로세스 : 프로그램을 실행하는 주체
- 스레드: 가벼운 프로세스

4. 시스템 보호 (System managemet)

## 컴퓨터 시스템의 구성

![image-20200917195647248](.\images\image-20200917195647248.png)

운영체제의 핵심 : kernel 

system call interface : OS kernel에 사용자가 요청의 전달하는 통로 or 사용자가 사용할 수 있는 기능들

![image-20200917195859769](.\images\image-20200917195859769.png)



## 운영체제의 구분

- 동시 사용자 수

  1. single-user system -> 한명만 쓰는 시스템, 개인용 장비 등에 사용

  2. Multi-user system - > 동시에 여러 사용자들이 시스템 사용, 서버/클러스터 장비 등에 사용

- 단일 작업

  1. 시스템 내에 하나의 작업(프로세스)만 존재 -> 하나의 프로그램 실행을 마친 뒤 다른 것 실행

  2. 운영체제의 구조가 간단

- 다중 작업

  1. 동시에 여러 작업(프로세스)의 수행 가능

  2. 운영체제의 기능 및 구조가 복잡 (Linux/Unix, Windows)

- 작업 수행 방식

  1. 순차 처리 (OS 없음 , ~1940년대까지 사용)
     - 하나씩 순차적으로 처리

  ![image-20200917203930605](.\images\image-20200917203930605.png)

  1. 배치 시스템 batch system (1950년대 ~ 1960년대)

     - 사용자의 요청 작업을 일정 시간 모아두었다가 한꺼번에 처리

     - 시스템 지향적

  2. 시분할 시스템 time sharing system (1960년대 ~1970년대)

     - 여러 사용자가 자원을 동시에 사용 -> OS가 파일 시스템 및 메모리 관리
     - 사용자 지향적 (대화형 시스템)
     - 응답시간 단축, 생산성 향상 / 통신 비용 증가, 개인 사용자 체감 속도 저하

  ![image-20200917204102905](.\images\image-20200917204102905.png)

  ![image-20200917204227102](.\images\image-20200917204227102.png)

  3. 개인 컴퓨팅 Personal Computing

     - 개인이 시스템 전체 독점
     - 빠른 응답시간/ 사용 효율 저조

  4. 병렬 처리 시스템 Parallel Processing System

     - 단일 시스템 내에서 둘 이상의 프로세서 사용 -> 동시에 둘 이상의 프로세스 지원

     - 메모리 등의 자원 공유
     - 성능 향상/ 신뢰성 향상

  ![image-20200917204603025](.\images\image-20200917204603025.png)

  5. 분산 처리 시스템 Distributed Processing System
     - 네트워크를 기반으로 구축된 병렬처리 시스템 (Loosely-coupled system)
     - 물리적인 분산, 통신망 이용한 상호 연결
     - 각각 운영체제 탑재한 다수의 범용 시스템으로 구성
     - 분산 운영체제를 통해 하나의 프로그램, 자원처럼 사용 가능

  ![image-20200917204824160](.\images\image-20200917204824160.png)

  6. 실시간 시스템 Real-time Systems

     - 작업 처리에 제한 시간(deadline)을 갖는 시스템

       - 제한 시간 내에 서비스를 제공하는 것이 자원 활용 효율보다 중요

     - 작업의 종류

       - Hard real-time task
         - 시간 제약을 지키지 못하는 경우 시스템에 치명적 영향(발전소,제어,무기)
       - soft real-time task

       - Non real-time task

## 운영체제의 구조

- 커널
  - os의 핵심 부분( 메모리 상주) -> 가장 빈번하게 사용되는 기능들 담당
  - 시스템 관리 (processor, memory, ETC)
  - 핵, 관리자 프로그램, 상주 프로그램, 제어프로그램 등과 같은 의미
-  유틸리티
  - 비상주 프로그램
  - UI등 서비스 프로그램

![image-20200917205553127](.\images\image-20200917205553127.png)

- 단일 구조 운영체제
  - 커널안에 모든 기능을 넣어 놓은 구조
  - 장점 : 커널 내 모듈간 직접 통신(효율적 자원관리) / 단점: 커널의 거대화 (유지보수 힘듬)

![image-20200917205734348](.\images\image-20200917205734348.png)

- 계층 구조 운영체제
  - 장점 : 모듈화 (계층간 검증 및 수정 용의), 설계 및 구현의 단순화
  - 단점 : 단일구조 대비 성능 저하

![image-20200917205854559](.\images\image-20200917205854559.png)

- 마이크로 커널 구조

  - 커널 크기 최소화 (필수 기능만 포함, 기타 기능은 사용자의 영역에서 처리)

  ![image-20200917205947092](.\images\image-20200917205947092.png)

## 운영체제의 기능

- 프로세스 관리
  - 프로세스 : 커널에 등록된 실행 단위 (실행중인 프로그램)
  - 사용자 요청/프로그램의 수행 주체(entity)
  - os의 프로세스 관리 기능
    - 생성/삭제, 상태관리
    - 자원 할당
    - 프로세스 간 통신 및 동기화
    - 교착상태(deadlock) 해결
- 프로세서 관리
  - 프로세서 = CPU
  - 프로그램을 실행하는 핵심 자원
  - 프로세스 스케줄링(시스템 내의 프로세스 처리 순서 결정)
  - 프로세서 할당 관리 (프로세서들에 대한 프로세서 할당)
- 메모리 관리
  - 주기억장치 = 메모리(DRAM)
  - Multi-user, Multi-tasking 시스템
    - 프로세스에 대한 메모리 할당 및 회수
    - 메모리 여유 공간 관리
    - 각 프로세스의 할당 메모리 영역 접근 보호
  - 메모리 할당 방법(scheme)
    - 전체 적재
    - 일부 적재
- 입출력 관리
  - 입출력 과정 -> os를 반드시 거쳐야 함
- 파일 관리
  - 파일: 논리적 데이터 저장 단위
  - 사용자 및 시스템의 파일 관리
  - 디렉토릭 구조 지원
- 보조 기억 장치 및 기타 주변장치 관리