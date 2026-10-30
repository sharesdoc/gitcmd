# Gitcmd.sh Quick Reference

## Menu Options

| # | Action | Command | Description |
|---|--------|---------|-------------|
| 1 | **status** | `git status` | Show working tree status |
| 2 | **fetch** | `git fetch [remote]` | Get updates from remotes |
| 3 | **log** | `git log [options]` | Show commit history |
| 4 | **diff** | `git diff branch..HEAD` | Compare with another branch |
| 5 | **checkout** | `git switch [branch]` | Switch/create branches |
| 6 | **merge** | `git merge [branch]` | Merge branches |
| 7 | **rebase** | `git rebase [branch]` | Rebase current branch |
| 8 | **pull** | `git pull [remote] [branch]` | Pull from remote |
| 9 | **push** | `git push [remote] [branch]` | Push to remote |
| 10| **stash** | `git stash [options]` | Stash changes |
| 11| **tag** | `git tag [name]` | Manage tags |
| 12| **delete branch** | `git branch -d [branch]` | Delete branches |
| 13| **delete file** | `git rm [file]` | Delete files |
| 14| **reset** | `git reset [commit]` | Reset branch |
| 15| **clean** | `git clean [options]` | Clean untracked files |
| h | **help** | - | Show help |
| q | **quit** | - | Exit program |

## Quick Demo Commands

### Basic Usage
```bash
# Run interactively
gitcmd

# Dry run (preview only)
export DRY_RUN=1
gitcmd

# Direct function calls (for testing)
source gitcmd.sh
DRY_RUN=1 wf_status
```

### Common Workflows

#### New Feature Development
```bash
gitcmd
# 5) checkout/switch → 3) new branch → "my-feature"
# ... make changes ...
gitcmd
# 1) status → 9) push → select branch → set upstream
```

#### Emergency Save Work
```bash
gitcmd
# 10) stash → 1) stash save → "emergency work"
# Later restore:
gitcmd
# 10) stash → 3) stash pop → select stash
```

#### Clean Repository
```bash
gitcmd
# 15) clean → select -n (preview) → -f (execute)
# 14) reset → 1) soft → HEAD~1 (uncommit last)
```

#### Branch Management
```bash
# Create and switch
gitcmd
# 5) checkout/switch → 1) local branch → "feature"

# Merge feature to main
gitcmd
# 6) merge → main → feature → choose options → cleanup

# Delete merged feature
gitcmd
# 12) delete branch → 1) local → "feature" → safe delete
```

### One-Liner Demos

```bash
# Check status
echo "" | DRY_RUN=1 wf_status

# Show log graph
echo -e "2 5" | DRY_RUN=1 wf_log

# Fetch all remotes
echo -e "origin\n--prune" | DRY_RUN=1 wf_fetch

# Create tag
echo -e "2\nv1.0.0\n" | DRY_RUN=1 wf_tag

# Stash with message
echo -e "1\nWIP: feature work" | DRY_RUN=1 wf_stash

# Clean preview
echo "-n" | DRY_RUN=1 wf_clean
```

## Safety Features

- ✅ **Dry Run Mode**: `export DRY_RUN=1` previews commands
- ✅ **Input Validation**: Prevents malformed commands
- ✅ **Confirmation Required**: All destructive operations need confirmation
- ✅ **Branch Protection**: Can't delete current branch
- ✅ **Backup Tags**: Creates backup before history rewrites

## Environment Variables

```bash
# Preview commands only
export DRY_RUN=1

# Enable/disable colors
export COLOR=1  # default
export COLOR=0  # disable colors
```

## Global Shortcuts

```bash
# Quick status
gitcmd → 1

# Quick log
gitcmd → 3 → 1 (oneline)

# Quick fetch
gitcmd → 2 → origin

# Quick push
gitcmd → 9 → origin → branch → -u

# Emergency stash
gitcmd → 10 → 1 → message

# Help
gitcmd → h

# Quit
gitcmd → q
```