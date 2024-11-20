# Git Command

## 1. git command
### git config
1. `git config --global user.name "user.name"` : 최초 1회만, 사용자 이름 설정
2. `git config --global user.email <user email>` : 최초 1회만, 사용자 이메일 설정
3. `git config --global commit.template .gitmessage.txt` : commit 템플릿 설정
### git init
1. 설명 : 기존 저장소를 git 저장소로 등록하기
2. 주의사항 : git 로컬 저장소 내에 또 다른 git 로컬 저장소를 만들지 말 것
    - git 저장소 안에 git 저장소가 있을 경우, 가장 바깥쪽의 git 저장소가 안쪽의 git 저장소의 변경사항을 추적할 수 없기 때문
    - **추가 공부 필요** : git submodule
### git status
1. 설명 : 파일 상태 확인하기 (cf. 하단 파일 상태 정리 문서)
### git add
1. 설명 : 변경사항이 있는 파일을 Staging area에 추가
2. `git add .` : 변경사항이 있는 모든 파일을 Staging area에 추가
### git commit
1. 설명 : Staging area에 있는 파일을 git 저장소에 기록, 해당 시점의 버전을 생성하고 변경 이력을 남기는 것
2. `git commit --amend` : 마지막 commit 변경
   * `git commit --amend -m` : commit 메세지 수정
   * 파일 수정 : Staging area에 있는 파일을 추가한 뒤 commit 가능
### git log
1. 설명 : comit 이력 조회
### git remote
1. `git remote -v` : 현재 local repository에 등록된 remote repository의 별칭과 URL을 확인
2. `git remote add <name> <remote repository url>` : remote repository를 \<name>이라는 별칭으로 추가
3. `git remote remove <name>` : 현재 로컬 저장소에 저장되었던 원격저장소 삭제
### git clone
1. 설명 : remote repository를 local repository로 복
2. `git clone <remote repository url>`

### git push
1. 설명 : remote repository에 commit을 업로드
2. `git push <name> <branch>` : 기본 사용법
3. `git push -u <name> <branch>` : 이후 push 시, 저장소 및 branch 지정 생략 가능
### git pull
1. 설명 : remote repository의 데이터를 가져온 후 local branch에 병합
### git remote update
1. 설명 : 원격 브랜치에 접근하기 위해 git remote를 갱신
## 2. git stages
### git의 3가지 영역
1.  Working Directory

    실제 작업 중인 파일들이 위치하는 영역
2.  Staging Area
    
    Working Directory에서 변경된 파일 중 다음 버전에 포함시킬 파일들을 선택적으로 추가하거나 제외할 수 있는 중간 준비 영역
3.  Repository
    
    버전(=commit)이력과 파일들이 영구적으로 저장되는 영역, 모든 버전과 변경 이력이 기록됨
    1. Local Repository
    2. Remote Repository

        코드와 버전 관리 이력을 온라인 상의 특정 위치 (git hub, remote repository)에 저장하여 여러 개발자가 협업하고 코드를 공유할 수 있는 저장 공간
       * 종류 : GitLab, GitHub, Bitbucket
### 파일의 상태
1. Untracked / Tracked
    
    Git에 의해 버전 관리되는 파일인지 / 아닌지
2. Staged / Unstaged
    
    commit의 대상인지 / 아닌지
3. Modified / Unmodified
    
    Working directory와 Stage 정보가 다른지 / 같은지  
