# Git Basics

```bash
# Clone single branch from repo
git clone -b <branch name> --single-branch <repo-url> <folder name>`
```

```bash
# Fetch another remote branch after a single-branch clone
git remote set-branches --add origin [remote-branch]
git fetch origin [remote-branch]:[local-branch]
```

```bash
# Squash and interactive rebase (can `pick`, `squash` or `reword` individual commits)
git rebase -i HEAD~[number of commits]

# Alternative to rebase from a different branch
git checkout [main-branch]
git pull origin
git checkout [dev-branch]
git rebase -i [main-branch]
```

```bash
# Force push to remote branch to overwrite history
git push origin branchName --force
```

```bash
# Force pull to local branch overwrite history
git fetch --all  # update all branches
git checkout -b backup-master
git reset --hard origin/master
```

```bash
# Prune remotes to remove any merged/deleted remote branches
git remote prune origin --dry-run
git branch -D $(git branch --merged)
```

```bash
# Undo n commits, soft is reversible, hard is irreversible
git reset --soft HEAD~n
git reset --hard HEAD~n
```

```bash
# Checking git diff history
git show HEAD~1
git show <COMMIT>
```

```bash
# Go to a specific commit and be in a detached state
git checkout -b <new-branch-name> <SHA1>
```