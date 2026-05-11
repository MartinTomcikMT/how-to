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

<img width="1210" height="309" alt="image" src="https://github.com/user-attachments/assets/f81b3d59-b8d9-4e45-bdef-8559b6fecb30" />

The second issue was that I created the repository on GitHub with an initial `README.md`, but I did not pull the remote changes before performing other Git operations locally.  

<img width="761" height="248" alt="image" src="https://github.com/user-attachments/assets/606e3f50-5b74-4b85-b9e3-a024063be5c9" />

I tried to rebase origin:

<img width="1099" height="515" alt="image" src="https://github.com/user-attachments/assets/fbde823d-8c53-4f5a-896f-f497a4131d54" />


After that, I renamed the branch from `master` to `main`, attempted `git pull --rebase origin main`, aborted the rebase, and finally executed a reset to `origin/main`.

The relevant entries in `git reflog` were:

```text
5d28336 (HEAD -> main) HEAD@{0}: rebase (abort): returning to refs/heads/main
2a7296b (origin/main) HEAD@{1}: pull --rebase origin main (start): checkout 2a7296b6b786c1304661585285b650a34682758b
5d28336 (HEAD -> main) HEAD@{2}: Branch: renamed refs/heads/master to refs/heads/main
5d28336 (HEAD -> main) HEAD@{4}: commit: Fix gitignore and remove venv
69be9ca HEAD@{5}: commit: Add gitignore and remove virtual environment
54cc209 HEAD@{6}: commit (initial): Create new repository for project Python Learning Library
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
