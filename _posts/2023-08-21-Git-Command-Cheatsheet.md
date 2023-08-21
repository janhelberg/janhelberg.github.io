---
title:  Git Command Cheatsheet
#author: Jan Helberg
date:   2023-08-21 00:15:00 +0200
categories: [Software, Git]
tags: [Software, Git, Cheatsheet]
---
## Configuration
Set the Git configuration values, Note there are three levels that can be passed in the commands

Level: Local\
Command: --local\
Location: .git/config

Level: Global\
Command: --global\
Location: C:\Users\\\\.gitconfig

Level: System\
Command: --system\
Location: C:\Documents and Settings\All Users\Application Data\Git\config
```powershell
# Configure user information
git config --global user.name "Peter Pan"
git config --global user.email "Peter@pan.com"

# Check current configuration
git config --list
```

## Repository Creation
```powershell
# Initialize a new repository
git init

# Clone a repository
git clone <repository_url>
```

## Branches 
```powershell
# Create a new branch
git checkout -b <new_branch_name>

# Switch to an existing branch
git checkout <branch_name>

# List all branches
git branch

# Delete a branch
git branch -d <branch_name>

# Merge changes from another branch
git merge <other_branch_name>
```

## Remote Operations 
```powershell
# Add a remote repository
git remote add <remote_name> <repository_url>

# List remote repositories
git remote -v

# Fetch remote changes
git fetch <remote_name>

# Pull and merge changes from a remote branch
git pull <remote_name> <branch_name>

# Push changes to a remote repository
git push <remote_name> <branch_name>
```

## Undoing Changes
```powershell
# Discard changes in a working directory
git checkout -- <file_name>

# Unstage changes from the staging area
git restore --staged <file_name>

# Amend the last commit
git commit --amend

# Revert a commit (create a new commit that undoes the changes)
git revert <commit_hash>
```

## Tagging
```powershell
# Create a lightweight tag
git tag <tag_name>

# Create an annotated tag with a message
git tag -a <tag_name> -m "Tag message"

# List all tags
git tag

# Push tags to a remote repository
git push origin --tags
```