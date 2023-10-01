## remote

```bash
git clone URL #cloning
git remote -v #shows all the remote URLs
git remote add newName URL #기존의 origin 이라는 remote 이름을 변경(newName)한다.
```

### Inspecting

```bash
git remote
git remote show
git remote show origin #origin에 관련된 정보를 확인
```

### Syncing with remotes

- `fetch` : server의 history를 local에 받아오지만 local의 Head는 여전히 기존의 것을 바라본다.
- `pull` : server의 history를 local에 받아오고 local의 내용과 merge 한다.

```bash
git fetch #pulls down all the data from remote
git fetch origin #same as the above
git fetch origin master #pulls down only master branch
git pull #fetch and merge
git pull --rebase #use rebase when pulling instead of merge
git push
git push origin master
```

### Renaming or Removing

```bash
git remote rename sec second
git remote remove second
```
