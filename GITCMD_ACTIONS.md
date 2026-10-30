# Gitcmd.sh - All Actions and Demo Commands

## Overview
Gitcmd.sh is an interactive Git helper that provides 15 main actions plus help functionality. Each action can be accessed through the menu or tested in dry-run mode.

## Menu Actions

### 1. **status** - `wf_status`
**Purpose**: Show current repository status
**Demo Command**:
```bash
# Interactive
gitcmd
# Choose option 1

# Direct function call (dry run)
source gitcmd.sh
DRY_RUN=1 wf_status
```
**Generated Command**: `git status`

---

### 2. **fetch** - `wf_fetch`
**Purpose**: Fetch updates from remote repositories
**Demo Commands**:
```bash
# Interactive with dry run preview
echo -e "origin\n--prune\ny" | DRY_RUN=1 wf_fetch
```
**Generated Commands**:
- `git fetch --all --prune` (when no remotes or choose all)
- `git fetch --prune origin` (specific remote with prune)
- `git fetch origin` (basic fetch)

**Available Options**:
- `--prune`: Remove remote-tracking references that no longer exist
- `--tags`: Fetch all tags

---

### 3. **log** - `wf_log`
**Purpose**: Show commit history with customizable options
**Demo Commands**:
```bash
# Interactive with multiple options
echo -e "1 2 3 4 5" | DRY_RUN=1 wf_log

# Simple oneline log
echo "1" | DRY_RUN=1 wf_log

# Graph view with last 30 commits
echo -e "2 5" | DRY_RUN=1 wf_log
```
**Generated Commands**:
- `git log --oneline --graph --decorate --all -n 30` (all options)
- `git log --oneline` (basic oneline)
- `git log --graph -n 30` (graph with limit)

**Available Options**:
- `--oneline`: Compact commit format
- `--graph`: Show ASCII commit tree
- `--decorate`: Show branch/tag names
- `--all`: Show all branches
- `-n 30`: Limit to 30 commits

---

### 4. **diff** - `wf_diff`
**Purpose**: Show differences between branches
**Demo Commands**:
```bash
# Interactive - will show branch selection menu
echo "1" | DRY_RUN=1 wf_diff  # Select first branch
```
**Generated Commands**:
- `git diff main..HEAD` (compare with main branch)
- `git diff origin/main..HEAD` (compare with remote branch)

**Process**:
1. Select target branch to diff against
2. Shows current HEAD vs selected branch

---

### 5. **checkout/switch** - `wf_checkout`
**Purpose**: Switch between branches or create new ones
**Demo Commands**:
```bash
# Switch to existing local branch
echo -e "1\nmain" | DRY_RUN=1 wf_checkout

# Create and track remote branch
echo -e "2\norigin/main\nfeature-branch\ny" | DRY_RUN=1 wf_checkout

# Create new branch from existing
echo -e "3\nmain\nnew-feature\ny" | DRY_RUN=1 wf_checkout
```
**Generated Commands**:
- `git switch main` (switch to local branch)
- `git switch -c feature-branch --track origin/main` (track remote)
- `git switch main && git switch -c new-feature` (create new branch)

**Options**:
1. **Local branch**: Switch to existing local branch
2. **Remote branch**: Create local tracking branch from remote
3. **New branch**: Create new branch from base branch

---

### 6. **merge** - `wf_merge`
**Purpose**: Merge one branch into another
**Demo Commands**:
```bash
# Interactive merge with options
echo -e "main\nfeature\n1 2" | DRY_RUN=1 wf_merge
```
**Generated Commands**:
- `git switch main && git merge --no-ff --no-commit feature`

**Process**:
1. Select TARGET branch (receives changes)
2. Select SOURCE branch (to be merged)
3. Choose merge options

**Available Options**:
- `--no-ff`: Create merge commit even for fast-forward
- `--ff-only`: Only allow fast-forward merges
- `--squash`: Merge changes without commit
- `--no-commit`: Merge but don't create commit

**Post-merge**: Option to delete source branch (local/remote)

---

### 7. **rebase** - `wf_rebase`
**Purpose**: Rebase one branch onto another
**Demo Commands**:
```bash
# Interactive rebase
echo -e "feature\nmain\n1" | DRY_RUN=1 wf_rebase
```
**Generated Commands**:
- `git switch feature && git rebase --interactive main`

**Process**:
1. Select WORK branch (to be rebased)
2. Select ONTO branch (new base)
3. Choose rebase options

**Available Options**:
- `--interactive`: Interactive rebase
- `--autosquash`: Auto-squash fixup commits
- `--rebase-merges`: Preserve merge commits

---

### 8. **pull** - `wf_pull`
**Purpose**: Pull changes from remote repository
**Demo Commands**:
```bash
# Pull with rebase
echo -e "origin\nmain\n1" | DRY_RUN=1 wf_pull
```
**Generated Commands**:
- `git pull --rebase origin main`

**Process**:
1. Select remote repository
2. Select remote branch
3. Choose pull options

**Available Options**:
- `--rebase`: Rebase instead of merge
- `--ff-only`: Only allow fast-forward
- `--no-rebase`: Force merge (default)

---

### 9. **push** - `wf_push`
**Purpose**: Push local changes to remote repository
**Demo Commands**:
```bash
# Push with upstream tracking
echo -e "origin\nfeature\n1" | DRY_RUN=1 wf_push

# Force push with lease
echo -e "origin\nmain\n2" | DRY_RUN=1 wf_push
```
**Generated Commands**:
- `git push -u origin feature` (set upstream)
- `git push --force-with-lease origin main` (safe force push)
- `git push origin --tags` (push all tags)

**Available Options**:
- `-u`: Set upstream tracking
- `--force-with-lease`: Safe force push
- `--tags`: Push all tags

---

### 10. **stash** - `wf_stash`
**Purpose**: Stash and restore working changes
**Demo Commands**:
```bash
# Save stash with message
echo -e "1\nWork in progress" | DRY_RUN=1 wf_stash

# Apply latest stash
echo -e "4\n1" | DRY_RUN=1 wf_stash

# Pop stash (apply and remove)
echo -e "3\n1" | DRY_RUN=1 wf_stash

# Drop specific stash
echo -e "5\n1" | DRY_RUN=1 wf_stash

# List stashes
echo "2" | DRY_RUN=1 wf_stash
```
**Generated Commands**:
- `git stash push -m "Work in progress"`
- `git stash apply stash@{0}`
- `git stash pop stash@{0}`
- `git stash drop stash@{0}`
- `git stash list`

**Operations**:
1. **Save**: Push current changes to stash
2. **List**: Show all stashes
3. **Pop**: Apply and remove latest stash
4. **Apply**: Apply stash without removing
5. **Drop**: Remove specific stash

---

### 11. **tag** - `wf_tag`
**Purpose**: Manage Git tags
**Demo Commands**:
```bash
# Create tag with message
echo -e "2\nv1.0.0\nHEAD" | DRY_RUN=1 wf_tag

# Delete local tag
echo -e "3\nv1.0.0" | DRY_RUN=1 wf_tag

# Push specific tag
echo -e "4\norigin\nv1.0.0" | DRY_RUN=1 wf_tag

# Push all tags
echo -e "4\norigin\n" | DRY_RUN=1 wf_tag

# List all tags
echo "1" | DRY_RUN=1 wf_tag
```
**Generated Commands**:
- `git tag "v1.0.0" "HEAD"`
- `git tag -d "v1.0.0"`
- `git push origin "v1.0.0"`
- `git push origin --tags`
- `git tag --list`

**Operations**:
1. **List**: Show all tags
2. **Create**: Create new tag
3. **Delete**: Delete local tag
4. **Push**: Push tag(s) to remote

---

### 12. **delete branch** - `wf_delete_branch`
**Purpose**: Delete local and/or remote branches
**Demo Commands**:
```bash
# Delete local branch only
echo -e "1\nfeature\n1" | DRY_RUN=1 wf_delete_branch

# Delete remote branch only
echo -e "2\norigin/feature" | DRY_RUN=1 wf_delete_branch

# Delete both local and remote
echo -e "3\nfeature\ny\norigin\ny" | DRY_RUN=1 wf_delete_branch
```
**Generated Commands**:
- `git branch -d "feature"` (safe delete)
- `git branch -D "feature"` (force delete)
- `git push origin --delete "feature"` (remote delete)

**Options**:
1. **Local**: Delete local branch only
2. **Remote**: Delete remote branch only
3. **Local + Remote**: Delete both

**Delete Modes**:
- `-d (safe)`: Only delete if merged
- `-D (force)`: Force delete unmerged branch

---

### 13. **delete file** - `wf_delete_file`
**Purpose**: Delete files from Git (normal or complete removal)
**Demo Commands**:
```bash
# Normal file deletion (staged)
echo -e "1\n1\nconfig.yml" | DRY_RUN=1 wf_delete_file

# Delete file but keep in working directory
echo -e "1\n1\nconfig.yml" | DRY_RUN=1 wf_delete_file

# Complete removal from history (dangerous)
echo -e "2\n2\nsecret.txt\ny\ny\ny" | DRY_RUN=1 wf_delete_file
```
**Generated Commands**:
- `git rm -- "config.yml"` (stage deletion)
- `git rm --cached -- "config.yml"` (remove from Git, keep file)
- `git filter-repo --path "secret.txt" --invert-paths` (complete history rewrite)

**Options**:
1. **Normal delete**: `git rm` (staged deletion)
2. **Purge from history**: `git filter-repo` (complete removal)

**Git rm options**:
- `--cached`: Keep file in working directory
- `-r`: Recursive deletion for directories

**⚠️ Warning**: History rewrite changes all commit hashes and requires force push

---

### 14. **reset** - `wf_reset`
**Purpose**: Reset current branch to previous state
**Demo Commands**:
```bash
# Soft reset (keep changes staged)
echo -e "1\nHEAD~1" | DRY_RUN=1 wf_reset

# Mixed reset (unstage changes)
echo -e "2\nHEAD~1" | DRY_RUN=1 wf_reset

# Hard reset (discard all changes)
echo -e "3\nHEAD~1" | DRY_RUN=1 wf_reset
```
**Generated Commands**:
- `git reset --soft HEAD~1`
- `git reset --mixed HEAD~1`
- `git reset --hard HEAD~1`

**Reset Modes**:
1. **Soft**: Keep all changes staged
2. **Mixed**: Unstage changes (default)
3. **Hard**: Discard all changes (dangerous)

**Target Options**:
- `HEAD~1`: Previous commit
- `commit_hash`: Specific commit
- `branch_name`: Branch tip

---

### 15. **clean** - `wf_clean`
**Purpose**: Remove untracked files and directories
**Demo Commands**:
```bash
# Preview what would be deleted
echo "-n" | DRY_RUN=1 wf_clean

# Force delete untracked files
echo "-f" | DRY_RUN=1 wf_clean

# Delete files and directories
echo "-fd" | DRY_RUN=1 wf_clean

# Aggressive cleanup
echo "-fxd" | DRY_RUN=1 wf_clean
```
**Generated Commands**:
- `git clean -n` (dry run - preview)
- `git clean -f` (force delete files)
- `git clean -fd` (delete files and directories)
- `git clean -fxd` (aggressive - ignore .gitignore)

**Clean Options**:
- `-n`: Dry run - show what would be deleted
- `-f`: Force deletion
- `-d`: Delete directories
- `-x`: Delete ignored files too
- `-fxd`: Aggressive cleanup

**⚠️ Warning**: Clean operations are irreversible

---

### h) **help** - `print_help`
**Purpose**: Show help information
**Demo Commands**:
```bash
# Show help
echo "h" | DRY_RUN=1 timeout 1 ../gitcmd.sh 2>/dev/null || true

# Or call directly
source ../gitcmd.sh && print_help
```

---

### q) **quit**
**Purpose**: Exit gitcmd
**Demo Commands**:
```bash
# Quit immediately
echo "q" | timeout 1 ../gitcmd.sh 2>/dev/null || true
```

---

## Global Options

### DRY_RUN Mode
Preview commands without executing:
```bash
export DRY_RUN=1
gitcmd
```

### Environment Variables
- `DRY_RUN=1`: Preview mode only
- `COLOR=1`: Enable colored output (default when TTY)

## Usage Examples

### Complete Workflow Example
```bash
# 1. Start new feature
gitcmd
# Choose: 5) checkout/switch
# Choose: 3) new branch
# Base: main
# New name: my-feature

# 2. Work and commit changes
# ... (edit files)
git add .
git commit -m "Add new feature"

# 3. Push to remote
gitcmd
# Choose: 9) push
# Remote: origin
# Branch: my-feature
# Options: 1) -u

# 4. Create pull request (manual)
# ... (create PR on GitHub/GitLab)

# 5. After merge, cleanup
gitcmd
# Choose: 1) status (check clean)
# Choose: 5) checkout/switch
# Choose: 1) local branch
# Branch: main
git pull  # Update main branch
gitcmd
# Choose: 12) delete branch
# Scope: 3) local + remote
# Branch: my-feature
```

### Quick Status Check
```bash
# Quick status in dry run
DRY_RUN=1 wf_status

# Or interactive
gitcmd
# Choose: 1) status
```

### Emergency Stash
```bash
# Save current work
gitcmd
# Choose: 10) stash
# Choose: 1) stash save
# Message: "Emergency save"

# Later, restore work
gitcmd
# Choose: 10) stash
# Choose: 3) stash pop
# Select: stash@{0}
```