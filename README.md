# Git Commands & Troubleshooting Guide

## Table of Contents

1. Git Repository Basics
2. Checking Repository Information
3. Clone Repository
4. Understanding Local vs Remote Branches
5. Fork and Upstream Workflow
6. Synchronizing Fork with Upstream
7. Branch Management
8. Git Sync Commands
9. Rebase vs Merge
10. Git Divergence Checks
11. Branch Tracking
12. Creating a New Independent Repository
13. Git Mirroring (Clone All Branches)
14. Common Git Errors and Fixes
15. Troubleshooting Checklist
16. Useful Git Commands Reference

---

# 1. Git Repository Basics

## Check Current Repository

```bash
git remote -v
```

Example:

```bash
origin   https://github.com/Sar-py-05/vprofile.git (fetch)
origin   https://github.com/Sar-py-05/vprofile.git (push)
```

---

## Check Current Branch

```bash
git branch
```

Example:

```bash
* main
```

---

## Check All Branches

```bash
git branch -a
```

Shows:

* Local branches
* Remote branches
* Tracking branches

---

# 2. Checking Repository Information

## Check Repository Status

```bash
git status
```

---

## Check Commit History

```bash
git log --oneline -10
```

---

## Visual Commit Graph

```bash
git log --oneline --graph --decorate --all
```

---

# 3. Clone Repository

## Standard Clone

```bash
git clone https://github.com/devopshydclub/vprofile-project.git
```

---

## Clone via SSH

```bash
git clone git@github.com:devopshydclub/vprofile-project.git
```

---

# 4. Understanding Local vs Remote Branches

Many developers make this mistake:

Wrong:

```bash
git branches
```

Error:

```bash
git: 'branches' is not a git command
```

Correct:

```bash
git branch
```

---

### Local Branches

```bash
git branch
```

Output:

```bash
* main
```

---

### Remote Branches

```bash
git branch -r
```

Output:

```bash
origin/main
upstream/main
```

---

### All Branches

```bash
git branch -a
```

---

# 5. Fork and Upstream Workflow

## Verify Remotes

```bash
git remote -v
```

Example:

```bash
origin    https://github.com/Sar-py-05/repo.git
upstream  https://github.com/original/repo.git
```

---

## Add Upstream Repository

If missing:

```bash
git remote add upstream https://github.com/original/repo.git
```

Verify:

```bash
git remote -v
```

---

## Fetch Upstream Branches

```bash
git fetch upstream
```

---

# 6. Synchronizing Fork with Upstream

## Fetch Updates

```bash
git fetch upstream
```

---

## Checkout Main

```bash
git checkout main
```

---

## Pull Changes

```bash
git pull upstream main
```

---

## Push to Your Fork

```bash
git push origin main
```

---

# 7. Branch Management

## List All Branches

```bash
git branch -a
```

---

## Create Local Branch from Upstream

Example:

```bash
git checkout -b local upstream/local
```

or

```bash
git switch -c local upstream/local
```

---

## Switch Branches

```bash
git checkout local
```

or

```bash
git switch local
```

---

## Push New Branch

```bash
git push -u origin local
```

---

# 8. Git Sync Commands

## Fetch Updates from Origin and Upstream

```bash
git fetch origin
git fetch upstream
```

---

## Sync Main Branch

```bash
git checkout main
git pull upstream main
git push origin main
```

---

## Check Status

```bash
git status
```

---

## One-Liner Sync

```bash
git fetch upstream && git pull upstream main && git push origin main
```

---

# 9. Rebase vs Merge

## Option 1: Rebase (Recommended)

```bash
git pull --rebase origin main
git push origin main
```

Advantages:

* Cleaner history
* Linear commit chain

---

## Option 2: Merge

```bash
git pull origin main
git push origin main
```

Advantages:

* Preserves complete history

---

## Option 3: Force Push

Use cautiously.

```bash
git push origin main --force-with-lease
```

---

# 10. Git Divergence Checks

This was one of the most useful discoveries.

## Check Sync with Origin

```bash
git rev-list --left-right --count main...origin/main
```

---

### Output Interpretation

#### Fully Synced

```bash
0 0
```

Meaning:

```text
Local main: 0 commits ahead
Origin main: 0 commits ahead
```

---

#### Local Ahead

```bash
3 0
```

Meaning:

```text
Need git push
```

---

#### Remote Ahead

```bash
0 5
```

Meaning:

```text
Need git pull
```

---

#### Diverged

```bash
2 4
```

Meaning:

```text
Need merge/rebase
```

---

## Compare with Upstream

```bash
git rev-list --left-right --count main...upstream/main
```

Example:

```bash
7 0
```

Meaning:

```text
Your branch is 7 commits ahead of upstream
Upstream has no newer commits
```

---

# 11. Branch Tracking

Check branch tracking:

```bash
git branch -vv
```

Example:

```bash
* main  abc123 [origin/main]
```

---

# 12. Creating a New Independent Repository

## Step 1

Clone Original Repo

```bash
git clone https://github.com/devopshydclub/vprofile-project.git
cd vprofile-project
```

---

## Step 2

Create New Empty GitHub Repository

Do NOT initialize with:

* README
* .gitignore
* License

---

## Step 3

Update Remote URL

```bash
git remote set-url origin https://github.com/Sar-py-05/vprofile-project_devopshydclub.git
```

Verify:

```bash
git remote -v
```

---

## Step 4

Push Everything

```bash
git push -u origin --all
git push -u origin --tags
```

---

# 13. Git Mirroring (Clone All Branches)

## Clone Everything

```bash
git clone --mirror https://github.com/devopshydclub/vprofile-project.git vprofile-temp.git
cd vprofile-temp.git
```

---

## Push Everything

```bash
git push --mirror https://github.com/Sar-py-05/vprofile-project_devopshydclub.git
```

---

## Cleanup

```bash
cd ..
rm -rf vprofile-temp.git
```

---

# 14. Common Git Errors and Fixes

## Error: git branches

Wrong:

```bash
git branches
```

Correct:

```bash
git branch
```

---

## Error: upstream does not appear to be a git repository

Error:

```bash
fatal: 'upstream' does not appear to be a git repository
```

Cause:

```bash
upstream remote not configured
```

Fix:

```bash
git remote add upstream https://github.com/original/repo.git
```

---

## Error: Branch Not Found Locally

You fetched upstream but cannot checkout branch.

Fix:

```bash
git branch -a
```

Find remote branch:

```bash
upstream/local
```

Create local branch:

```bash
git checkout -b local upstream/local
```

---

## Error: Cannot See Remote Branches

Fetch first:

```bash
git fetch upstream
```

Then:

```bash
git branch -a
```

---

## Error: Local Repo Different from GitHub

Check:

```bash
git rev-list --left-right --count main...origin/main
```

Interpret results and sync accordingly.

---

# 15. Troubleshooting Checklist

Before Deploying to EC2:

```bash
git status
```

Must be clean.

---

```bash
git branch
```

Verify correct branch.

---

```bash
git remote -v
```

Verify remotes.

---

```bash
git fetch origin
```

Refresh origin.

---

```bash
git fetch upstream
```

Refresh upstream.

---

```bash
git rev-list --left-right --count main...origin/main
```

Verify sync.

---

```bash
git push origin main
```

Push pending changes.

---

# 16. Useful Git Commands Reference

```bash
git status
git branch
git branch -a
git branch -vv
git checkout main
git checkout -b local upstream/local
git switch local
git fetch origin
git fetch upstream
git pull upstream main
git push origin main
git remote -v
git remote add upstream URL
git log --oneline
git log --graph --decorate --all
git rev-list --left-right --count main...origin/main
git rev-list --left-right --count main...upstream/main
git clone URL
git clone --mirror URL
git push --mirror URL
git push --all
git push --tags
```

---

# Lessons Learned

1. `git branch` shows only local branches.
2. `git branch -a` shows local and remote branches.
3. `git fetch` updates references but does not modify working files.
4. `git pull` performs fetch + merge/rebase.
5. Always configure `upstream` when working with forks.
6. Use `git rev-list --left-right --count` before deciding whether to pull or push.
7. Never use `--force` unless you fully understand the consequences.
8. Verify synchronization before deployments, CI/CD runs, and production releases.
9. Use `git clone --mirror` when you need all branches and tags.
10. Keep `origin` (your fork) and `upstream` (source repo) clearly separated.

```
```
