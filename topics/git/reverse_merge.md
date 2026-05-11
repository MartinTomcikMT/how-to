# 🛠️ GIT RECOVERY - LOST LOCAL FILES AFTER RESET TO ORIGIN/MAIN

Short description:
This how-to describes a real Git incident that happened during the creation of a new GitHub repository for my Python project. Due to an incorrect workflow and the use of a destructive Git command, my local files disappeared from the current branch. This guide explains what happened, how the issue was diagnosed, how the data was recovered, and what best practices can prevent similar problems in the future.

Technology:
- Git
- GitHub
- WSL (Windows Subsystem for Linux)
- Python
- Virtual Environment (venv)

---

## 💣 ISSUE

During the creation of a new repository from my local machine, several Git-related problems occurred.

The first issue was that I forgot to create a `.gitignore` file, so my Python virtual environment (`venv`) was accidentally committed.

The second issue was that I created the repository on GitHub with an initial `README.md`, but I did not pull the remote changes before performing other Git operations locally.

The local commit history looked like this:

```text
e6ae977 Create new repository for project Python Learning Library
c067599 Add gitignore and remove virtual environment
e9773db Fix gitignore and remove venv
```

After that, I renamed the branch from `master` to `main`, attempted `git pull --rebase origin main`, aborted the rebase, and finally executed a reset to `origin/main`.

The relevant entries in `git reflog` were:

```text
16d09bf HEAD@{0}: reset: moving to origin/main
e9773db HEAD@{1}: rebase (abort): returning to refs/heads/main
16d09bf HEAD@{2}: rebase (start): checkout origin/main
e9773db HEAD@{3}: rebase (abort): returning to refs/heads/main
16d09bf HEAD@{4}: pull --rebase origin main (start)
e9773db HEAD@{5}: Branch: renamed refs/heads/master to refs/heads/main
e9773db HEAD@{7}: commit: Fix gitignore and remove venv
c067599 HEAD@{8}: commit: Add gitignore and remove virtual environment
e6ae977 HEAD@{9}: commit (initial): Create new repository for project Python Learning Library
```

As a result, my local files disappeared from the current branch and were not visible on GitHub either. At first, it looked as if all work had been permanently lost.

---

## 🔎 DIAGNOSTICS

To understand what happened, I checked the current Git state and history.

### Commands Used for Diagnosis

```bash
git status
git remote -v
git branch -vv
git log --oneline --graph --all
git reflog
```

The most important diagnostic command was `git reflog`, because the missing commits were no longer visible in the standard branch history, but they were still recorded in the reflog.

---

## ✅ SOLUTION

### Step 1: Check Current State

```bash
git status
```

### Step 2: Check Visible History

```bash
git log --oneline --graph --all
```

### Step 3: Inspect Reflog

```bash
git reflog
```

### Step 4: Create Rescue Branch

```bash
git branch rescue-lost-work e9773db
```

### Step 5: Switch to Rescue Branch

```bash
git checkout rescue-lost-work
```

### Step 6: Verify Recovery

```bash
ls
git status
git log --oneline
```

All missing files were restored.

### Step 7: Recover to Main Branch

```bash
git checkout main
git merge rescue-lost-work
```

The safest approach was to recover the data into a separate branch first and only then decide how to integrate it.

---

## 💪 WHAT I LEARNED

### Skills Practiced

- Git recovery
- Git reflog usage
- Branch recovery
- Understanding local vs remote history
- Safe repository initialization
- `.gitignore` management
- Python virtual environment cleanup
- Reset and rebase troubleshooting

### Useful Commands

```bash
git status
git remote -v
git branch -vv
git log --oneline --graph --all
git reflog
git branch rescue-lost-work <commit_hash>
git checkout rescue-lost-work
git cherry-pick <commit_hash>
git merge <branch_name>
```

### Key Lessons

- `git reset --hard origin/main` moves the branch pointer and overwrites the working tree.
- `git reflog` records branch movements and can help recover commits that seem lost.
- Lost work is not always permanently lost if the commit was created.

---

## ☑️ BEST PRACTICES

### Clone Existing Repositories First

```bash
git clone git@github.com:username/repository.git
cd repository
```

### Create `.gitignore` Before First Commit

```gitignore
.venv/
venv/
__pycache__/
*.pyc
.env
```

### Always Check Status Before Risky Operations

```bash
git status
```

### Create Backup Branches Before Destructive Commands

```bash
git branch backup-before-reset
```

### Use `git fetch` to Inspect Remote Changes

```bash
git fetch origin
git log --oneline --graph --all
```

### Be Very Careful with Destructive Commands

Risky commands:
- `git reset --hard`
- `git clean -fd`
- `git push --force`

### Learn and Use `git reflog`

`git reflog` should be part of every developer's troubleshooting toolkit.
