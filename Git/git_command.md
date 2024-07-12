# Git Command

## 1. git command
### git config

### git init

### git status

### git add

### git commit

### git push

### git log

### git remote add \<name> \<remote repository url>

### git clone \<remote repository url>

### git pull

### git push
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
