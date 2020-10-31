### 주의 할점

- 반드시 개인 branch 생성 한 후 작업 (backend, frontend 에서 작업 x)
- git merge 명령어 사용 x
- Merge Request 요청 엉뚱한 곳으로 안보내게 주의

```bash
# git 시작
$ git init

# 원격 저장소랑 연걸
$ git remote add origin https://lab.ssafy.com/s03-webmobile1-sub2/s03p12d108.git

# 현재 로컬 및 원격 저장소에 연결된 branch 확인
$ git branch -a

# 아마 원격 저장소에 있는 branch들이 반영이 안되어있을 것이므로 update
$ git remote update

# 원격 저장소에 있는 backend branch를 로컬에 복사 및 이동
$ git checkout -t origin/backend

# 중요!! backend branch에서 개인의 작업 branch 생성
$ git checkout -b feature/backend0721

# 개인 작업

# lab.ssafy에 개인이 작업한 branch push
$ git add .
$ git commit -m "0721"
$ git push origin feature/backend0721

# lab.ssafy 홈페이지에서 Merge Request 요청 
# backend팀은 backend branch에 요청하고, frontend팀은 backend branch에 요청
# git merge 명령어는 대참사를 유발하므로 조심
```



### 기본 명령어

```bash
# develop라는 새로운 branch 생성
$ git checkout -b develop

# develop라는 특정 branch로 부터 새로운 branch 생성
$ git checkout -b develop origin/develop

# 다시 master로 돌아올 때
$ git checkout master

# branch 목록 확인
$ git branch

# 원격 저장소의 브랜치 리스트 조회
$ git branch -r

# 로컬, 원격 저장소의 브랜치 리스트 조회
$ git branch -a

# 원격 저장소를 최신 상태로 로컬 저장소에 갱신
# 처음에는 기존의 branch가 없는 경우가 있는데, 이 명령어를 통해 기존 원격 저장소에 있는
# branch 들을 가져옴
$ git remote update

# git flow 생성, 생성하면 develop라는 새로운 branch 가 생성된다
$ git flow init

# 생성된 develop branch를 원격 저장소에 등록
# 로컬에서 생성된 develop라는 branch를 원격에 등록하겠다
$ git push origin develop

# 원격 저장소의 branch 가져오기
# 원격 저장소의 backend를 가져오고 싶다.
$ git checkout -t origin/backend
```



