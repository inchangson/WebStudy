### Git

#### 버전 관리
- Git은 소스코드 관리용 분산형 버전 관리 시스템
- 파일이 달라지지 않으면 새로 저장하지 않음
- 이전 상태의 파일에 대한 링크 저장 
- 데이터를 스냅샷의 스트림처럼 취급
- 프로젝트의 모든 히스토리가 로컬 디스크에 저장됨(속도 빠름)

#### 세 가지 상태
- Commit -> 데이터가 로컬 데이터베이스에 저장됨
- Modify -> 수정한 파일을 로컬 데이터베이스에 커밋하지 않은 것
- Staged -> 수정한 파일을 곧 커밋할 것이라는 표시

#### Remote 저장소
- 인터넷이나 네트워크 어딘가에 있는 저장소
- git remote add <단축이름> <url>로 리모트 저장소 추가 가능

#### 작업 트리 & 인덱스 + Commit
- 저장소 <-> 인덱스 <-> 작업 트리
- Git의 커밋 작업은 작업 트리에 있는 변경 내용을 저장소에 바로 기록하는 것이 아니라 인덱스에 파일 상태를 기록(Staging)하는 것
- 저장소에 변경 사항 기록을 위해선 변경 사항들이 인덱스 안에 존재해야함

#### Push
- 원격 저장소로 변경된 파일을 업로드하는 것 
- 원격 저장소 내 변경 이력 업로드 & 원격 저장소와 로컬 저장소가 동일 상태가 됨

#### Clone
- 원격 저장소를 복제

#### fetch vs pull
- git fetch를 통해 리모트 저장소에 있는 데이터를 가지고 올 수 있음(로컬에 없는 파일)
- fetch의 경우 자동으로 merge하지 않음
- 로컬에서 정리 후 수동으로 merge 필요
- git pull의 경우 자동으로 리모트 브랜치와 로컬 브랜치를 merge

#### Branch
- 종류
    1. 통합 브랜치
        1. 언제든지 배포할 수 있는 버전을 만드는 브랜치
        2. 안정적인 상태 우선 / 모든 기능이 정상적으로 동작하는 상태
        3. 일반적으로 master 브랜치를 통합 브랜치로 사용
    2. 토픽 브랜치
        1. 기능 추가나 버그 수정과 같은 단위 작업을 위한 브랜치
- branch 명령으로 브랜치 생성 가능
- checkout 명령으로 브랜치 이동 가능(HEAD)
- git checkout -b 하면 브랜치 생성 + head 이동 가능

#### Merge
- git merge ~ 명령어를 통해 HEAD가 가리키는 브랜치에 통합
- 이전 버전으로 돌아가기 -> 브랜치 생성 + 커밋 정리 후(stashing, cleaning) -> 이전 브랜치로 checkout
##### Hotfix
- Master branch에서 발생한 Hotfix branch의 경우 -> Hotfix branch 사용 & 운영 환경 배포(master branch로 checkout & git merge hotfix 
- 별다른 merge 과정 없이 master branch에서 hotfix branch로 단순히 최신 커밋으로 이동하게 됨(fast-forward)
- git branch -d hotfix 명령 필요
##### merge 과정
- 각 branch가 가리키는 커밋 두 개 & 공통 조상 하나 사용(3way merge)
- 결과를 별도의 커밋으로 만들고 그 branch가 해당 커밋을 가리키도록 이동시킴
##### conflict
- 자동으로 merge과정 실패
- git status로 merge 실패 파일 확인
##### 관리 
- git branch 명령어로 브랜치 목록 확인 가능
- -v 붙이면 브랜치마다 마지막 커밋 메세지까지 확인 가능
- *이 없으면 이미 merge해서 삭제해도 상관없는 branch

#### Rebase
- merge를 사용하는 경우 뿌리가 여러개로 나뉨 -> 히스토리 찾기 복잡함
- 두 개의 공통 base를 가진 branch에서 한 branch를 다른 하나의 branch의 최신 커밋으로 base를 옮기는 작업 -> ex) bugfix 브랜치를 master 브랜치에 rebase하면 master 브랜치 이후로 이동(충돌 수정 필요)
- 작업하고 있는 branch의 최신 변경 사항 반영 장점 & 커밋 이력 x -> 히스토리 깔끔해짐
- merge의 경우 충돌 발생을 한번만 처리하면 됨 -> rebase의 경우 branch 각각의 커밋마다 충돌 해결해야함
- 커밋 이력이 변경되기 때문에 정확한 이력을 남겨야할 때에는 사용 불가능

#### Stash
- 작업을 하던 중에 다른 요청이 들어온 경우 -> 브랜치를 변경해야함
- 아직 마무리하지 않은 작업을 스택에 잠시 저장

#### Clean
- Untracked 파일들을 삭제
- -n 명령으로 삭제 대상 확인 가능


### VSCode에서 git 사용
- Git init | git clone으로 사용
- Changes에서 +로 Stage처리 가능(Changes에서 Unstage처리 가능)
- 체크 표시로 commit 가능
- 왼쪽 하단의 master를 통해 branch 생성 가능
- branch에서 push -> PR -> master에서 pull