## Local Changes

### Discarding local changes

```bash
git restore --staged file.txt #unstaging a staged file
git restore file.txt #unmodifying a modified file
git restore . #unmodifying all modified files in the directory
git clean -fd #모든 untracked files 제거
```

### Restoring file from certain commit

```bash
git restore --source=hash file.txt #해당 hash 코드의 파일 resotre 한다.
git restore --source=HEAD~2 file.txt #HEAD로 부터 2개전 커밋으로 restore 한다.
```

### 마지막 commit 수정하기

- `git commit --amend`

### git reset

- `git reset --soft HEAD` : 커밋을 삭제하며 `staging area` 에 변경사항을 저장한다.
- `git reset --mixed HEAD` : 기본 옵션, 변경된 내용을 `working directory` 로 옮긴다.
- `git reset --hard HEAD` : 커밋을 삭제하며 로컬의 파일까지 모두 삭제 한다.

### git reflog

```bash
git reflog
git reset --hard hash
```

### Revert

히스토리를 수정하지 않고, 새로운 커밋을 만든다.

```bash
git revert hash #해당 hash코드의 취소된 커밋이 생성된다.
git revert HEAD~1
gut revert --no-commit hash #revert 커밋 없이 staging area에 추가한다.
```

### Interactive Rebasing

혼자 작업 할떄 커밋 히스토리를 변경할 때 유용하다.

```bash
git rebase -i HEAD~2 #지정한 HEAD 부터 rebase 해준다.
git rebase --continue
git rebase --abort
```
