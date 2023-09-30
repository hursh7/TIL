### fast-forward merge

history 내역이 남지 않는다.

- `git merge featureA` : master에 featureA를 merge 한다.
- `git merge --no-ff featureA` : merge commit 이 만들어진다. (history 내역이 남는다.)

### Three-way merges

서로 다른 브랜치에 공통되는 base branch를 기점으로 충돌을 최소화 시키는 방법

브랜치간의 병합을 진행할 때, base를 기준으로 어떠한 브랜치의 파일이 수정되었는지
확인할 수 있어서 충돌의 문제를 해결하는데 효과적이다.

- `git merge --squash featureA` #suqash merge, only one commit
- `git merge --no-ff featureA` #creates a merge commit
- `git merge --abort` : merge 취소
- `git merge --continue` : conflict 해결 후 merge 계속하기
- `git mergetool` #opens merge tool

### rebase

![git rebase](images/rebase.png)

- `git rebase master` : 현재 브랜치를 master 브랜치에 rebase 한다.
  (다시 master 브랜치에서 해당 브랜치를 머지한다.)
- `git rebase --onto master service ui` : service 브랜치에서 파생된 ui 브랜치만 master 브랜치에 rebase 한다.
  ![git rebase_onto](images/rebase_onto.png)

### Merge Tool

p4merge 툴 사용하여 merge 하기

`.gitconfig`

```bash
[merge]
    tool = vscode
[mergetool]
	keepBackup = false
[mergetool "vscode"]
    cmd = code --wait $MERGED
[mergetool "p4merge"]
    path = "/Applications/p4merge.app/Contents/MacOS/p4merge"
```
