# HIgh-level Mechanism

- Language support !!

## Monitor

- **공유 데이터와 Critical Section의 집합**
- Conditional variabel
  - wait(), signal() operations

![image-20200925170943943](.\images\image-20200925170943943.png)

- Enter queue (진입 큐)
  - 모니터 내의 procedure(function) 수 만큼 존재
- Mutual exclusion
  - 모니터 내에는 항상 하나의 프로세스만 진입 가능
- Information hiding (정보 은폐)
  - 공유 데이터는 모니터 내의 프로세스만 접근 가능
- Condition queue (조건 큐)
  - 모니터 내의 특정 이벤트를 기다리는 프로세스가 대기
- Signaler queue (신호 제공자 큐)
  - 모니터에 항상 하나의 신호제공자 큐가 존재
  - signal() 명령을 실행한 프로세스가 임시 대기

## 자원 할당 문제

![image-20200925171312876](.\images\image-20200925171312876.png)

- 비유 : 책방
  - 자원 R : 한권 밖에 없는 책
  - requestR(): 책 요청
  - releaseR() : 책 반납
  - condition queue (R_Free): 책을 빌리기 위해 대기하는 공간
  - signaler queue (1개): 대기실에서 대기하는 인원을 깨  우는 것 
- 자원 R을 한번에 한명만 쓸 수 있을 때 처리 문제
  - requestR(): 자원을 요청
  - releaseR() : 자원을 반납

![image-20200925172002874](.\images\image-20200925172002874.png)

## 자원 할당 시나리오

- 자원 R 할당 가능
- Monitor에 할당 없음

1. 프로세스 Pj가 모니터 안에서 자원 R을 요청

![image-20200925172139320](.\images\image-20200925172139320.png)

2. 프로세스 Pk,Pm이 자원할당 요청

   - 자원 R이 Pj에게 할당 됨
   - 프로세스 Pk, Pm이 R 요청

   - 자원이 없기 때문에 (Pj가 점유하고있기 떄문) R_Free에서 대기

![image-20200925172300915](.\images\image-20200925172300915.png)

3. Pj가 R 반환
   - R_Free.signal() 호출에 의해 Pk가 wakeup
   - Pj는 signaler queue에서 대기

![image-20200925172608572](.\images\image-20200925172608572.png)

4. 자원 R이 Pk에게 할당 됨
   - Pj가 모니터 안으로 돌아와서 남은 작업 수행

![image-20200925172759237](.\images\image-20200925172759237.png)

## Producer-Consumer Problem

![image-20200925173047974](.\images\image-20200925173047974.png)

 ## Dining philosopher problem

- 5명의 철학자
- 철학자들은 생각하는 일, 스파게티 먹는 일만 반복함
- 공유 자원: 스파게티, 포크
- 스파게티를 먹기 위해서는 좌우 포크 2개 모두 들어야 함

![image-20200925173647421](.\images\image-20200925173647421.png)

- Dining philosopher problem
  - pickup(i) : 포크를 집어든다
  - putdown(i): 포크를 내려놓는다  -> 두가지의 프로시저

## 구현

numforks : 철학자들이 사용할 수 있는 포크 수

![image-20200925175503636](.\images\image-20200925175503636.png)

```pseudocode
monitor dining_philosophers;
# numforks : 철학자들이 사용할 수 있는 포크 수
var numforks: array[0..4] for integer, 
	ready : array[0..4] of condition
	i  	: integer

# 모니터 이기 때문에 한번에 한명만 입장 가능
procedure pickup(me);
var me : integer;
begin
	# 내 포크가 2개가 아니면 기다려라
	if (numForks[me] != 2) then ready ready[me].wait();
	# 2개가 가능하다면 집는데, 내 오른쪽과 왼쪾 사람은 쓸수 있는 포크가 1개 줌
	numForks[right(me)] <- numForks[right(me)] -1 ;
	numForks[left(me)] <- numForks[left(me)] -1;
end;

procedure putdown(me);
var me : integer;
begin
	# 포크 내려놓기 때문에 좌우 포크 갯수 한개 는다
	numForks[right(me)] <- numForks[right(me)] + 1 ;
	numForks[left(me)] <- numForks[left(me)] + 1;
	# 내 오른쪽 사람의 포크가 2개인데 만일 기다리고 있었다면 signal() 로 깨워줌
	if (numForks[right(me)] = 2) then ready[right(me)].signal();
	# 왼쪽도 마찬가지
	if (numForks[left(me)] = 2) then ready[left (me)].signal();
end;

begin
	for i <- 0 to 4 do
		numForks[i] <- 2
	end
end
```



## Monitor 장단점

- 장점
  - 사용이 쉽다
  - Deadlock 등 error 발생 가능성이 낮음
- 단점
  - 지원하는 언어에서만 사용 가능 (Monitor)
  - 컴파일러가 os를 이해하고 있어야 함
    - Critical section 접근을 위한 코드 생성

