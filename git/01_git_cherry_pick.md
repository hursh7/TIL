# Git Cherry Pick

git cherry-pick은 필요한 commit만 골라서 가져오는 명령어이다.

쉽게 말해 다른 브랜치에 있는 commit들 중에서 원하는, 필요한 commit을 지금 내 브랜치에 가져와 commit을 하는 것이다.
(해당 commit을 branch끼리 옮기는 게 아니라 전체 history를 따지면 **새로운 commit**이 늘어나는 것이다.)

**Cherry-pick 사용이 권장 될 때**

Cherry-Pick은 유용한 기능이지만, 같은 commit이 여러 번 쌓이는 일이 발생할 수 있기 때문에 보통은 일반적인 merge를 사용하는 것이 권장된다.

Cherry-Pick이 유용하게 사용되는 상황

- 팀으로 협업 할 때
- 빠르게 버그를 수정하고 Main branch에 반영 할 때
- 반영되지 않은 PR
  - 실수로 pull request를 merge하기 전에 닫아버렸을 때, cherry-pick을 통해 해당 commit을 가져올 수 있다.

**Cherry-pick 명령어**

```jsx
git cherry-pick <commit여섯자리번호>

// 여러 커밋을 한번에
git cherry-pick 76ae30ef 13af32cc

// 커밋 범위의 첫번째.. 마지막
git cherry-pick 555f8b4..480b6bb
```

**Conflict 발생 시**

```jsx
// cherry-pick을 중단하고 싶을 때
git cherry-pick -abort

// -continue 옵션으로 충돌을 수정하여 반영하기
vi $file_path # 파일 수정
git add $file_path
git cherry-pick -continue
```

**merge commit을 Cherry-pick 할 때**

```jsx
git cherry-pick -m 1 <merge_commit_hash>
```
