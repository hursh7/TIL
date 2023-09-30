# Git log

```bash
git log #list of commits
git log --patch #shows the difference introduced in each commit
git log -p #same as --patch
git log --state #abbreviated states for each commit
git log --oneline #oneline
git log --oneline --reverse #oneline, from the oldest to the newest
```

## Formatting

```bash
git log --pretty=oneline #same as --oneline
git log --pretty=format:"%h - %an %ar %s" #formatting
git log --pretty=format:"%h %s" --graph #show graph
git log --graph --all --pretty=format:'%C(yellow)[%ad]%C(reset) %C(green)[%h]%C(reset) | %C(white)%s %C(bold red){{%an}}%C(reset) %C(blue)%d%C(reset)' --date=short
```
