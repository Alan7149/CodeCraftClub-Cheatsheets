# 🌿 Git Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Git version control quick reference.

---

## Setup & Config

```bash
git config --global user.name "Alan Babu"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --list
```

## Start a Repo

```bash
git init                          # new repo here
git clone <url>                   # copy a remote repo
git clone <url> myfolder
```

## Daily Workflow

```bash
git status                        # what changed
git add file.txt                  # stage one file
git add .                         # stage everything
git commit -m "message"           # commit staged changes
git commit -am "message"          # add tracked + commit
git log --oneline --graph --all   # history
git diff                          # unstaged changes
git diff --staged                 # staged changes
```

## Branching

```bash
git branch                        # list branches
git branch feature                # create branch
git checkout feature              # switch to it
git switch feature                # modern switch
git checkout -b feature           # create + switch
git switch -c feature             # modern create + switch
git branch -d feature             # delete (merged)
git branch -D feature             # force delete
git branch -m old new             # rename
```

## Merging & Rebasing

```bash
git merge feature                 # merge into current branch
git rebase main                   # replay commits onto main
git rebase --abort                # bail out of a rebase
# Resolve conflicts, then:
git add <file>
git rebase --continue
```

## Remotes

```bash
git remote -v                     # list remotes
git remote add origin <url>
git push -u origin main           # first push (sets upstream)
git push                          # push commits
git pull                          # fetch + merge
git fetch                         # download, don't merge
git push origin --delete branch   # delete remote branch
```

## Undo & Fix

```bash
git restore file.txt              # discard unstaged changes
git restore --staged file.txt     # unstage (keep changes)
git reset --soft HEAD~1           # undo commit, keep staged
git reset --mixed HEAD~1          # undo commit, keep changes
git reset --hard HEAD~1           # undo commit + DISCARD changes ⚠️
git revert <hash>                 # new commit that undoes another
git commit --amend -m "new msg"   # fix last commit message
```

## Stashing

```bash
git stash                         # save changes aside
git stash list
git stash pop                     # reapply + remove
git stash apply                   # reapply, keep stash
git stash drop
```

## Inspecting

```bash
git log --oneline -10
git show <hash>                   # details of a commit
git blame file.txt                # who changed each line
git diff main..feature            # compare branches
git reflog                        # history of HEAD (recover lost commits)
```

## Tags

```bash
git tag v1.0.0
git tag -a v1.0.0 -m "release"
git push origin v1.0.0
git push origin --tags
```

## .gitignore

```gitignore
node_modules/
.env
__pycache__/
*.log
dist/
.DS_Store
```

## Common Recovery

```bash
git checkout -- .                 # discard ALL unstaged changes ⚠️
git clean -fd                     # delete untracked files/dirs ⚠️
git cherry-pick <hash>            # copy a commit to current branch
git reset --hard origin/main      # match remote exactly ⚠️
```

> ⚠️ = destructive, can lose work. Double-check before running.

---

[🔝 Back to README](../README.md)
