
# Git Cheatsheet (macOS + Branches)

This cheatsheet covers initializing a repo, ignoring `.DS_Store`, working with branches, committing/pushing, merging, and helpful diagnostics.

---

## 📦 Setup & Clone
```bash
# Clone an existing GitHub repo
git clone https://github.com/<your_user>/<your_repo>.git
cd <your_repo>

# If starting from a local folder that isn't a repo yet
git init
git remote add origin https://github.com/<your_user>/<your_repo>.git

# Set default branch to main (optional)
git branch -M main
git push -u origin main
```

---

## 🧹 Ignore macOS .DS_Store (recommended)
```bash
# Add ignore rules
echo ".DS_Store" >> .gitignore
echo "**/.DS_Store" >> .gitignore
git add .gitignore
git commit -m "Ignore .DS_Store files"

# If .DS_Store is already tracked, remove it from Git index (keep local file)
git rm --cached .DS_Store
# Remove all tracked occurrences if there are many
find . -name .DS_Store -print0 | xargs -0 git rm --cached

git commit -m "Remove .DS_Store from tracking"
```

> **Global ignore (optional):**
```bash
git config --global core.excludesfile ~/.gitignore_global
echo ".DS_Store" >> ~/.gitignore_global
echo "**/.DS_Store" >> ~/.gitignore_global
```

---

## ✍️ Basic Workflow: Add → Commit → Push
```bash
# See what's changed
git status

# Stage everything
git add .

# Commit with a message
git commit -m "Your commit message"

# Push current branch to GitHub (first push sets upstream)
git push -u origin <branch_name>

# Subsequent pushes/pulls (after upstream is set)
git push
git pull
```

> **If you forget `-m` and Git opens Vim:**
- Save & exit: `Esc` then `:wq` then Enter  
- Abort commit: `Esc` then `:q!` then Enter  
- Prefer nano as editor: `git config --global core.editor "nano"`

---

## 🌿 Branching
```bash
# Create and switch to a new branch
git checkout -b feature-my-change
# or (newer)
git switch -c feature-my-change

# Switch between branches
git checkout master        # or main
git switch master          # or main
git switch branchtest1

# List branches
git branch         # local
git branch -r      # remote
git branch -vv     # with tracking info
```

---

## 🧳 Stash (temporarily save changes)
```bash
# Stash unstaged/working changes
git stash

# Switch branches safely, do something else...

# Bring stashed changes back
git stash pop
```

---

## 🔀 Merge a branch into master/main
```bash
# Ensure you're on the target branch
git switch master          # or: git switch main

# Update local master/main from remote (optional, recommended)
git pull origin master     # or: git pull origin main

# Merge your feature branch into master/main
git merge branchtest1

# Resolve conflicts if prompted, then:
git add <fixed-files>
git commit

# Push the updated master/main
git push origin master     # or: git push origin main
```

---

## 🌐 Set/Verify Remote
```bash
# Show remotes
git remote -v

# Set remote
git remote add origin https://github.com/<your_user>/<your_repo>.git

# Change remote (if URL is wrong)
git remote set-url origin https://github.com/<your_user>/<your_repo>.git
```

---

## 🧭 Helpful Diagnostics
```bash
git status                                  # current branch + staged/unstaged changes
git log --oneline --graph --decorate --all  # compact commit history
git diff                                    # see unstaged changes
git diff --staged                           # see staged changes
```

---

## 🗑️ Clean up branches (after merge)
```bash
# Delete a local branch
git branch -d branchtest1

# Delete the remote branch on GitHub
git push origin --delete branchtest1
```

---

## 🛑 Common Errors & Fixes

**“Your local changes would be overwritten by checkout”**
```bash
# Option A: commit the changes first
git add .
git commit -m "Save work before switching"

# Option B: stash temporarily
git stash
git switch <other-branch>
# later: git stash pop
```

**Accidentally tracked `.DS_Store` even after adding `.gitignore`**
```bash
git rm --cached .DS_Store
git commit -m "Stop tracking .DS_Store"
```

**“The current branch has no upstream branch” (first push)**
```bash
git push -u origin <branch_name>
```
