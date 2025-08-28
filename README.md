# Git Cheat Sheet

## Initial Setup

### Configure Git
```bash
# Set your name and email
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Check configuration
git config --list
git config user.name
git config user.email
```

### Initialize Repository
```bash
# Create new repository
git init

# Clone existing repository
git clone https://github.com/username/repository.git
git clone git@github.com:username/repository.git

# Clone specific branch
git clone -b branch-name --single-branch https://github.com/username/repository.git
```

## Basic Workflow

### Check Status and Changes
```bash
# Check repository status
git status

# See differences in working directory
git diff

# See differences in staged files
git diff --staged
git diff --cached

# Show commit history
git log
git log --oneline
git log --graph --oneline --all
```

### Staging Changes
```bash
# Stage specific file
git add filename.txt

# Stage multiple files
git add file1.txt file2.txt

# Stage all changes
git add .

# Stage all modified files (not new files)
git add -u

# Interactive staging
git add -p filename.txt

# Unstage file
git reset HEAD filename.txt
git restore --staged filename.txt  # newer Git versions
```

### Committing Changes
```bash
# Commit staged changes
git commit -m "Commit message"

# Commit all modified files (skip staging)
git commit -am "Commit message"

# Amend last commit
git commit --amend -m "Updated commit message"

# Commit with detailed message
git commit  # Opens editor for multi-line message
```

## Branching

### Create and Switch Branches
```bash
# List branches
git branch                    # Local branches
git branch -r                 # Remote branches
git branch -a                 # All branches

# Create new branch
git branch new-branch-name

# Create and switch to new branch
git checkout -b new-branch-name
git switch -c new-branch-name  # newer Git versions

# Switch to existing branch
git checkout branch-name
git switch branch-name         # newer Git versions

# Delete branch
git branch -d branch-name      # Safe delete
git branch -D branch-name      # Force delete
```

### Branch Naming Conventions
```bash
# Feature branches
git checkout -b feature/user-authentication
git checkout -b feature/shopping-cart

# Bug fix branches
git checkout -b bugfix/login-error
git checkout -b hotfix/critical-security-issue

# Release branches
git checkout -b release/v2.1.0
```

## Remote Repositories

### Working with Remotes
```bash
# List remote repositories
git remote -v

# Add remote repository
git remote add origin https://github.com/username/repository.git

# Change remote URL
git remote set-url origin https://github.com/username/new-repository.git

# Remove remote
git remote remove origin
```

### Fetch, Pull, and Push
```bash
# Fetch changes from remote
git fetch origin

# Pull changes from remote (fetch + merge)
git pull origin main
git pull                       # If upstream is set

# Push changes to remote
git push origin main
git push                       # If upstream is set

# Push new branch to remote
git push -u origin new-branch  # Sets upstream tracking
git push origin new-branch

# Delete remote branch
git push origin --delete branch-name
```

## Merging and Rebasing

### Merging Branches
```bash
# Merge branch into current branch
git merge feature-branch

# Merge with no fast-forward (creates merge commit)
git merge --no-ff feature-branch

# Abort merge
git merge --abort
```

### Rebasing
```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (last 3 commits)
git rebase -i HEAD~3

# Continue rebase after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort
```

## Undoing Changes

### Working Directory Changes
```bash
# Discard changes in working directory
git checkout -- filename.txt
git restore filename.txt       # newer Git versions

# Discard all working directory changes
git checkout .
git restore .                  # newer Git versions
```

### Staged Changes
```bash
# Unstage file (keep changes in working directory)
git reset HEAD filename.txt
git restore --staged filename.txt  # newer Git versions

# Unstage all files
git reset HEAD
```

### Committed Changes
```bash
# Undo last commit (keep changes staged)
git reset --soft HEAD~1

# Undo last commit (keep changes in working directory)
git reset --mixed HEAD~1
git reset HEAD~1               # default is --mixed

# Undo last commit (discard all changes)
git reset --hard HEAD~1

# Revert commit (creates new commit that undoes changes)
git revert commit-hash
git revert HEAD                # Revert last commit
```

## Stashing

### Save and Apply Changes
```bash
# Stash current changes
git stash
git stash push -m "Work in progress"

# List stashes
git stash list

# Apply most recent stash
git stash apply
git stash pop                  # Apply and remove from stash

# Apply specific stash
git stash apply stash@{2}

# Drop stash
git stash drop stash@{1}

# Clear all stashes
git stash clear
```

## Viewing History and Information

### Log and History
```bash
# Show commit history
git log
git log --oneline
git log --graph
git log --graph --oneline --all

# Show changes in each commit
git log -p

# Show commits by author
git log --author="John Doe"

# Show commits in date range
git log --since="2 weeks ago"
git log --until="2023-12-31"

# Show file history
git log -- filename.txt
```

### Show and Diff
```bash
# Show commit details
git show commit-hash
git show HEAD                  # Show last commit

# Compare branches
git diff main..feature-branch

# Compare commits
git diff commit1..commit2

# Show file at specific commit
git show commit-hash:filename.txt
```

## Advanced Commands

### Cherry Pick
```bash
# Apply specific commit to current branch
git cherry-pick commit-hash

# Cherry pick multiple commits
git cherry-pick commit1 commit2

# Cherry pick without committing
git cherry-pick --no-commit commit-hash
```

### Blame and Bisect
```bash
# Show who changed each line of file
git blame filename.txt

# Find commit that introduced bug (binary search)
git bisect start
git bisect bad                 # Current commit is bad
git bisect good commit-hash    # Known good commit
# Test each commit Git suggests, then mark as good/bad
git bisect good                # Current commit is good
git bisect bad                 # Current commit is bad
git bisect reset               # End bisect session
```

### Clean
```bash
# Show untracked files that would be removed
git clean -n

# Remove untracked files
git clean -f

# Remove untracked files and directories
git clean -fd

# Remove ignored files too
git clean -fx
```

## SSH Keys Setup

### Generate SSH Key
```bash
# Generate Ed25519 key (recommended)
ssh-keygen -t ed25519 -C "your_email@example.com"

# Generate RSA key
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# Add key to SSH agent
ssh-add ~/.ssh/id_ed25519

# Copy public key to clipboard (macOS)
pbcopy < ~/.ssh/id_ed25519.pub

# Copy public key to clipboard (Windows)
clip < ~/.ssh/id_ed25519.pub

# Test SSH connection
ssh -T git@github.com
```

## Git Aliases (Optional)

### Useful Aliases
```bash
# Set up common aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

## Troubleshooting

### Common Issues
```bash
# Merge conflicts
# 1. Open conflicted files and resolve conflicts
# 2. Stage resolved files: git add filename.txt
# 3. Complete merge: git commit

# Forgot to add files to last commit
git add forgotten-file.txt
git commit --amend --no-edit

# Wrong commit message
git commit --amend -m "Correct message"

# Committed to wrong branch
git log --oneline              # Note commit hash
git reset --hard HEAD~1        # Remove commit from current branch
git checkout correct-branch
git cherry-pick commit-hash    # Apply commit to correct branch

# Push rejected (non-fast-forward)
git pull origin main           # Pull latest changes
git push origin main           # Push again

# Accidentally committed large file
git rm --cached large-file.txt
git commit --amend -m "Remove large file"
```

### Recovery Commands
```bash
# Find lost commits
git reflog

# Recover deleted branch
git checkout -b recovered-branch commit-hash

# Find when file was deleted
git log --oneline --follow -- deleted-file.txt
```

## Best Practices

### Commit Messages
- Use imperative mood: "Add feature" not "Added feature"
- First line under 50 characters
- Blank line before detailed explanation
- Explain what and why, not how

### Workflow
- Commit early and often
- Write meaningful commit messages
- Keep commits focused (one logical change per commit)
- Use branches for features and experiments
- Regularly sync with remote repository
- Review changes before committing (`git diff --staged`)

### Branch Management
- Use descriptive branch names
- Delete merged branches
- Keep main/master branch stable
- Create pull requests for code review
