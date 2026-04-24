# 🛠️ [REVERSE AND MERGE WITH DATA LOST]

- short description of how to manual
- which technology

# 💣 [ISSUE]

- During the creation of new repositary from local machine occured a problem with commiting data. First issue was that I forgot to create .gitignore file for venv on local machine and second was that I created repository on GitHub with README.md, but I didn`t pull before other actions. Situation:
  
  e6ae977 Create new repositary for project Python Learning Library  
  c067599 Add gitignore and remove virtual environment  
  e9773db Fix gitignore and remove venv

- After that I changed master branch to main, tried pull --rebase withoit success. I aborted rebase and finaly made reset on origin/main. In reflog I found these activities:

  16d09bf HEAD@{0}: reset: moving to origin/main  
  e9773db HEAD@{1}: rebase (abort): returning to refs/heads/main   
  16d09bf HEAD@{2}: rebase (start): checkout origin/main  
  e9773db HEAD@{3}: rebase (abort): returning to refs/heads/main  
  16d09bf HEAD@{4}: pull --rebase origin main (start)  
  e9773db HEAD@{5}: Branch: renamed refs/heads/master to refs/heads/main  
  e9773db HEAD@{7}: commit: Fix gitignore and remove venv  
  c067599 HEAD@{8}: commit: Add gitignore and remove virtual environment  
  e6ae977 HEAD@{9}: commit (initial): Create new repositary for project Python Learning Library

# 🔎 [DIAGNOSTICS]

- what exactly was checked

# ✅ [SOLUTION]

- step by step solution

# 💪 [WHAT I LEARNED]

- whoch skills
- useful commands

# ☑️ [BEST PRACTICES]

- ideas to prevent issue next time
- what is good for and what is risk

# 🚈 [NEXT]

- advantages for future

