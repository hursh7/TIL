# git

### git의 환경

git은 총 3가지의 작업 환경으로 나뉘어져 있다.

첫 번째는 프로젝트의 파일들을 수정하고 작업하는 **working directory**,

두 번째는 버전 히스토리에 저장할 준비가 되어있는 파일들을 옮겨 놓는 **staging area,**

마지막으로 버전의 히스토리를 가지고 있는 **.git directory(git repository)** 가 존재한다.

### 커밋하기

- git init : 해당 폴더를 git의 관리 하에 놓이게 한다.
- rm -rf .git : .git 폴더 제거 (다시 git의 관리 밖으로 설정)
- git status : 아직 담기지 않은 파일들을 표기한다.
- git add -A : 모든 파일을 담는다.

- git commit -m "message" : 메세지와 함께 stage에 커밋한다.
- vi editor가 실행돼서 명령어를 칠 수 없다면 :q 를 입력하여 빠져나온다.

### 과거의 상태로 돌아가기

git log : 시점의 일련번호(앞의 6자리) 확인하기

- **과감한 방법 - 복원할 여지 없이 완전히 지운다.**
  돌아가고자 하는 시점의 일련번호 여섯자리를 복사하여, git reset 000000 —hard를 입력한다.
- **신중한 방법 - 취소할 시점을 찾는다.**
  git revert 000000 를 입력하면 vi 창이 뜨고, 그대로 저장하겠다는 :wq 명령어를 입력한다.

### Branch

- git branch another : master의 현 상태를 그대로 가져오는 'another'라는 브랜치를 만든다.
- git branch : 현존하는 master와 branch들을 볼 수 있다.
- git checkout another : 'another' 브랜치로 이동한다.
- git checkout master : master로 돌아온다.
- git merge another : 브랜치(another) 변경사항을 master와 병합해준다.
- git rebase another : 다른 브랜치들의 분기 들이 한 줄로 합쳐진다.
- git branch -d another : 다른 브랜치(another)를 삭제한다.
- git push origin another : 원격 저장소에 another branch를 생성한다.
- git push -D origin another : 원격 저장소의 anohter branch를 삭제한다.
- git checkout -b another : branch를 만들고 해당 branch로 이동한다.
- git checkout -b another origin/another : 로컬에 another 브랜치를 만들어서 origin원격의 antoher 브랜치의 내용을 받아오고, 해당 브랜치로 이동한다.

### gitignore

- tracking 하고 싶지 않은 파일들을 지정해두는 곳.
- \*.log : .log 확장자를 가진 모든 파일, build/ : build directory 안의 파일.

### gitdiff

- git diff : working directory의 이전 파일과 현재 파일 상태 비교.
- git diff —staged, git diff —cached : staging area에 있는 것을 확인하기.
- git diff -h : 다양한 방법으로 원하는 변경 사항 확인 가능.
- gitdiff를 VScode로 확인하기

### git fetch

git fetch 후 git status : github에서 다운 받아야 할 사항이 있는지 확인하기.

git pull origin(원격 명) master(로컬 브랜치 명) : 원격 저장소에서 로컬로 다운로드 하기.

## git stash

- git stash: 변경 내용 임시 저장하기.
- git stash list : 내가 stash 했던 내용 보기.
- git stash show
- git stash apply: 가장 최근 stash 가지고 오기.
- git stash drop: 가장 최근 stash 지우기.
- git stash drop stash@{1}: 특정 stash 지우기. (stash명은 stash list 참고.)
- git stash clear : 한번에 stash 모두 지우기

stash 는 계속 진행할수록 stack의 원리로 더 아래 순번으로 가게 되며 더 큰 index 숫자를 갖게 된다. 그리고 **stash는 apply 했다고 해서 자동으로 지워 지지 않으므로**, stash list에 계속 남아 있게 된다. 때문에 stash apply 해서 가지고 온 작업들을 안전하게 commit 까지 했다면, **stash drop 혹은 stash clear로 지워주는 것이 좋다.**

### git stash 심화 명령어

- git stash pop : 가장 최근 stash를 적용하고 동시에 stack에서 지워 준다. (apply + drop)
- git stash pop stash@{1} : 특정 stash를 적용하고 동시에 stack에서 지워 준다.
- git stash save _stash이름_ : stash를 원하는 이름으로 정하기
