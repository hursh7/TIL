## git stash

working directory에 있는 파일을 stash stack 에 임시로 저장할 수 있다.

- `git stash`: 변경 내용 임시 저장하기.
- `git stash push -m "message"` : 이름 지정해서 임시 저장하기.
- `git stash --keep-index` : staging area에 작업하던 내용을 유지한 채로 stash.
- `git stash -u` : untracked 파일도 포함해서 stash.
- `git stash list` : 내가 stash 했던 내용 보기.
- `git stash show stash@{n}` : 해당 stash 확인.
- `git stash apply` : 가장 최근 stash 가지고 오기.
- `git stash drop` : 가장 최근 stash 지우기.
- `git stash drop stash@{1}` : 특정 stash 지우기. (stash명은 stash list 참고.)
- `git stash clear` : 한번에 stash 모두 지우기
- `git stash branch newBranch` : stash 하면서 적용된 새로운 브랜치를 만들며 이동한다.

stash 는 계속 진행할수록 stack의 원리로 더 아래 순번으로 가게 되며 더 큰 index 숫자를 갖게 된다. 그리고 **stash는 apply 했다고 해서 자동으로 지워 지지 않으므로**, stash list에 계속 남아 있게 된다. 때문에 stash apply 해서 가지고 온 작업들을 안전하게 commit 까지 했다면, **stash drop 혹은 stash clear로 지워주는 것이 좋다.**

### git stash 심화 명령어

- `git stash pop` : 가장 최근 stash를 적용하고 동시에 stack에서 지워 준다. (apply + drop)
- `git stash pop stash@{1}` : 특정 stash를 적용하고 동시에 stack에서 지워 준다.
- `git stash save _stash이름_` : stash를 원하는 이름으로 정하기
