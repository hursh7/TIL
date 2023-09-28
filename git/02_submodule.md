# Git SubModule

Git 저장소 안에 다른 Git 저장소를 디렉토리로 분리해 넣는 것이 `서브 모듈`이다. 다른 독립된 Git 저장소를 Clone 해서 내 Git 저장소 안에 포함할 수 있으며, 각 저장소의 커밋은 독립적으로 관리한다.

즉, `Parent repository` 에서 `Child repository`를 관리하기 위한 도구이다.

# git SubModule 명령어

외부 모듈을 현재 작업 디렉토리에 추가(clone)할 수 있다.

```bash
git submodule add {Git Repository URL} {Submodule Path Name}
```

`git status` 로 추가된 내용을 확인 후, commit 하면 된다.

```bash
git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   .gitmodules
	new file:   Submodule Path Name
```

- submodule이 추가되었다면 `git submodule status` 명령으로 추가된 모듈 목록을 볼 수 있다.

- Submodule을 초기화하고 업데이트하기

  ```bash
  git submodule init
  git submodule update
  ```

- 만약 Submodule 레포지터리를 최신 상태로 가져오려면 다음 명령어를 사용합니다.
  ```bash
  git submodule update --remote
  ```

# git SubModule 삭제 방법

- 먼저 `git submodule deinit` 명령으로 해당 submodule을 제거한다.

`git submodule deinit -f {Submodule Project directory}`

- 다음으로는 `rm` 명령으로 `.git/modules` 해당 디렉토리를 삭제한다.

`rm -rf .git/modules/{Submodule Project directory}`

- 그리고 git에서 해당 디렉토리를 제거해 준다.

`git rm -f {Submodule Project directory}`

- 마지막으로 `git commit`을 하면 외부 모듈을 삭제하여 커밋 한다.
