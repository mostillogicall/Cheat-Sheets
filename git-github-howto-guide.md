# Git & GitHub How-To Guide

*Practical workflows and step-by-step tutorials*

---

## Table of Contents

- [Starting a New Project](#starting-a-new-project)
- [Contributing to an Existing Project](#contributing-to-an-existing-project)
- [Daily Development Workflow](#daily-development-workflow)
- [Fixing Mistakes](#fixing-mistakes)
- [Working with Branches](#working-with-branches)
- [Handling Merge Conflicts](#handling-merge-conflicts)
- [Creating and Using Pull Requests](#creating-and-using-pull-requests)
- [Keeping Your Fork in Sync](#keeping-your-fork-in-sync)
- [Release Management](#release-management)
- [Advanced Workflows](#advanced-workflows)

---

## Starting a New Project

### Creating a Local Repository and Pushing to GitHub

**Step 1: Create the repository locally**

```bash
# Navigate to your project directory
cd my-project

# Initialize git
git init

# Create a README
echo "# My Project" > README.md

# Make your first commit
git add README.md
git commit -m "Initial commit"
```

**Step 2: Create repository on GitHub**

1. Go to GitHub and click the "+" icon → "New repository"
2. Name it (e.g., "my-project")
3. Don't initialize with README (you already have one)
4. Click "Create repository"

**Step 3: Connect local to remote**

```bash
# Add the remote
git remote add origin git@github.com:username/my-project.git

# Push your code
git push -u origin main
```

### Starting with a GitHub Repository First

**Step 1: Create on GitHub**

1. Go to GitHub → New repository
2. Name it and initialize with README
3. Click "Create repository"

**Step 2: Clone it locally**

```bash
git clone git@github.com:username/my-project.git
cd my-project

# Start working!
```

---

## Contributing to an Existing Project

### Method 1: Direct Collaboration (Team Member)

**Step 1: Clone the repository**

```bash
git clone git@github.com:team/project.git
cd project
```

**Step 2: Create a feature branch**

```bash
git checkout -b feature/my-feature
```

**Step 3: Make changes and commit**

```bash
# Edit files
git add .
git commit -m "Add my feature"
```

**Step 4: Push and create PR**

```bash
git push -u origin feature/my-feature
```

Then create a Pull Request on GitHub.

### Method 2: Fork Workflow (Open Source)

**Step 1: Fork on GitHub**

1. Go to the repository on GitHub
2. Click "Fork" in the top-right
3. Wait for the fork to complete

**Step 2: Clone your fork**

```bash
git clone git@github.com:yourusername/project.git
cd project
```

**Step 3: Add upstream remote**

```bash
git remote add upstream git@github.com:original-owner/project.git
```

**Step 4: Create feature branch**

```bash
git checkout -b feature/my-contribution
```

**Step 5: Make changes and push**

```bash
# Make your changes
git add .
git commit -m "Add my contribution"
git push -u origin feature/my-contribution
```

**Step 6: Create Pull Request**

1. Go to your fork on GitHub
2. Click "Compare & pull request"
3. Fill in the details
4. Submit the PR

---

## Daily Development Workflow

### The Standard Flow

**Morning: Start your day**

```bash
# Switch to main branch
git checkout main

# Get latest changes
git pull

# Create/switch to your feature branch
git checkout -b feature/new-thing
# or if it exists: git checkout feature/new-thing
```

**During the day: Save your work**

```bash
# Check what you've changed
git status

# Review your changes
git diff

# Stage specific files
git add file1.js file2.js

# Or stage everything
git add .

# Commit with a good message
git commit -m "Add user authentication logic"

# Push to remote (first time)
git push -u origin feature/new-thing

# Push to remote (subsequent times)
git push
```

**End of day: Clean up**

```bash
# Make sure everything is committed
git status

# Push any remaining work
git push
```

### Working on Multiple Features

```bash
# Start feature A
git checkout -b feature/feature-a
# Work on it...
git add .
git commit -m "Work on feature A"

# Need to switch to feature B
git checkout -b feature/feature-b
# Work on it...
git add .
git commit -m "Work on feature B"

# Back to feature A
git checkout feature/feature-a
# Continue working...
```

### Saving Work in Progress

If you need to switch branches but aren't ready to commit:

```bash
# Save your work temporarily
git stash save "WIP: working on login form"

# Switch branches
git checkout other-branch

# Do other work...

# Come back
git checkout original-branch

# Restore your work
git stash pop
```

---

## Fixing Mistakes

### I Made a Typo in My Last Commit Message

```bash
git commit --amend -m "Correct commit message"

# If already pushed (and you're the only one on the branch)
git push --force
```

### I Forgot to Add a File to My Last Commit

```bash
# Add the forgotten file
git add forgotten-file.js

# Amend the commit
git commit --amend --no-edit

# If already pushed
git push --force
```

### I Need to Undo My Last Commit (But Keep the Changes)

```bash
# Undo commit, keep changes staged
git reset --soft HEAD~1

# Undo commit, keep changes but unstage them
git reset --mixed HEAD~1

# Or simply
git reset HEAD~1
```

### I Need to Completely Undo My Last Commit

```bash
# WARNING: This deletes your changes!
git reset --hard HEAD~1
```

### I Committed to the Wrong Branch

```bash
# On wrong-branch with the commit you want to move
git log  # Note the commit hash

# Switch to the correct branch
git checkout correct-branch

# Apply the commit
git cherry-pick <commit-hash>

# Go back to wrong branch
git checkout wrong-branch

# Remove the commit
git reset --hard HEAD~1
```

### I Need to Undo a Commit That's Already Pushed

```bash
# Find the commit hash
git log --oneline

# Create a new commit that reverses it
git revert <commit-hash>

# Push the revert
git push
```

### I Accidentally Deleted a File

```bash
# If not yet committed
git restore deleted-file.js

# If committed, find when it existed
git log --all --full-history -- deleted-file.js

# Restore from that commit
git checkout <commit-hash> -- deleted-file.js
```

---

## Working with Branches

### Creating a Feature Branch

```bash
# Make sure you're starting from main
git checkout main
git pull

# Create and switch to new branch
git checkout -b feature/user-profile
```

### Keeping Your Branch Up to Date

**Method 1: Merge (preserves history)**

```bash
# On your feature branch
git checkout feature/user-profile

# Get latest main
git fetch origin

# Merge main into your branch
git merge origin/main

# Resolve any conflicts, then push
git push
```

**Method 2: Rebase (cleaner history)**

```bash
# On your feature branch
git checkout feature/user-profile

# Get latest changes
git fetch origin

# Rebase on top of main
git rebase origin/main

# If there are conflicts, fix them, then:
git add .
git rebase --continue

# Force push (your history changed)
git push --force
```

### Deleting Branches

```bash
# Delete local branch (after merging)
git branch -d feature/old-feature

# Force delete local branch
git branch -D feature/abandoned-feature

# Delete remote branch
git push origin --delete feature/old-feature
```

---

## Handling Merge Conflicts

### When Conflicts Happen

Conflicts occur when two branches modify the same lines of code. Here's how to resolve them:

**Step 1: Attempt the merge**

```bash
git checkout main
git pull
git checkout feature/my-feature
git merge main
# CONFLICT! Git will tell you which files have conflicts
```

**Step 2: Find the conflicts**

```bash
git status  # Shows conflicted files
```

**Step 3: Open conflicted files**

You'll see markers like this:

```
<<<<<<< HEAD
Your current branch's code
=======
The incoming code from main
>>>>>>> main
```

**Step 4: Resolve the conflict**

Edit the file to keep what you want:

```javascript
// Remove the markers and keep the right code
function myFunction() {
  // Keep this logic
  return result;
}
```

**Step 5: Mark as resolved and commit**

```bash
# After fixing all conflicts
git add .
git commit -m "Merge main into feature/my-feature"
git push
```

### Aborting a Merge

If things go wrong:

```bash
git merge --abort
```

---

## Creating and Using Pull Requests

### Creating a PR

**Step 1: Push your branch**

```bash
git push -u origin feature/my-feature
```

**Step 2: On GitHub**

1. Navigate to the repository
2. Click "Compare & pull request" (or go to Pull Requests → New)
3. Select base branch (usually `main`) and your feature branch
4. Write a clear title and description
5. Link related issues (e.g., "Closes #123")
6. Request reviewers if needed
7. Click "Create pull request"

### PR Description Template

```markdown
## Description
Brief description of what this PR does

## Changes
- Added feature X
- Fixed bug Y
- Updated documentation

## Testing
How to test these changes

## Screenshots (if applicable)
Add screenshots for UI changes

## Related Issues
Closes #123
```

### Updating a PR After Review

```bash
# Make requested changes
# Edit files...

# Commit and push
git add .
git commit -m "Address review comments"
git push

# The PR automatically updates!
```

### Merging a PR

On GitHub, you have options:

1. **Merge commit**: Keeps all commits (default)
2. **Squash and merge**: Combines all commits into one (cleaner history)
3. **Rebase and merge**: Replays commits on top of base branch

After merging:

```bash
# Switch to main and update
git checkout main
git pull

# Delete the feature branch
git branch -d feature/my-feature
git push origin --delete feature/my-feature
```

---

## Keeping Your Fork in Sync

### Initial Setup

```bash
# Add upstream remote (do this once)
git remote add upstream git@github.com:original-owner/project.git

# Verify
git remote -v
```

### Regular Syncing

```bash
# Fetch upstream changes
git fetch upstream

# Switch to your main branch
git checkout main

# Merge upstream changes
git merge upstream/main

# Push to your fork
git push origin main
```

### Syncing Your Feature Branch

```bash
# First sync main (as above)
git checkout main
git fetch upstream
git merge upstream/main
git push origin main

# Then update your feature branch
git checkout feature/my-feature
git merge main

# Or use rebase for cleaner history
git rebase main
```

---

## Release Management

### Creating a Release

**Step 1: Prepare the release**

```bash
# Switch to main and update
git checkout main
git pull

# Create release branch
git checkout -b release/v1.0.0
```

**Step 2: Update version numbers**

```bash
# Edit version files (package.json, VERSION, etc.)
# Update CHANGELOG.md

git add .
git commit -m "Bump version to 1.0.0"
```

**Step 3: Create and push tag**

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin release/v1.0.0
git push origin v1.0.0
```

**Step 4: Merge to main**

```bash
git checkout main
git merge release/v1.0.0
git push
```

**Step 5: Create GitHub Release**

1. Go to repository → Releases → Draft a new release
2. Choose your tag (v1.0.0)
3. Write release notes
4. Attach binaries if needed
5. Publish release

### Hotfixes

```bash
# Create hotfix branch from main
git checkout main
git checkout -b hotfix/critical-bug

# Fix the bug
# Edit files...
git add .
git commit -m "Fix critical security issue"

# Tag the hotfix
git tag -a v1.0.1 -m "Hotfix: security patch"

# Merge back to main
git checkout main
git merge hotfix/critical-bug
git push
git push origin v1.0.1

# Merge to develop if you have one
git checkout develop
git merge hotfix/critical-bug
git push
```

---

## Advanced Workflows

### Interactive Rebase (Cleaning Up History)

Use this before submitting a PR to clean up messy commits:

```bash
# Rebase last 5 commits
git rebase -i HEAD~5
```

You'll see an editor with options:

```
pick abc1234 First commit
pick def5678 Second commit
pick ghi9012 Oops, typo fix
pick jkl3456 Third commit

# Commands:
# p, pick = use commit
# r, reword = use commit, but edit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like squash, but discard commit message
# d, drop = remove commit
```

Example: squash the typo fix:

```
pick abc1234 First commit
pick def5678 Second commit
f ghi9012 Oops, typo fix
pick jkl3456 Third commit
```

Save and close, Git will combine the commits.

### Bisect (Finding When a Bug Was Introduced)

```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark a known good commit
git bisect good v1.0.0

# Git will checkout commits for you to test
# After testing each:
git bisect good  # if it works
git bisect bad   # if it's broken

# Git will eventually tell you which commit introduced the bug

# When done
git bisect reset
```

### Working with Submodules

```bash
# Add a submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Clone a repo with submodules
git clone --recursive https://github.com/user/repo.git

# Or if already cloned
git submodule init
git submodule update

# Update submodules
git submodule update --remote
```

### Git Worktrees (Multiple Working Directories)

```bash
# Create a worktree for a hotfix
git worktree add ../project-hotfix hotfix/critical-bug

# Work in that directory
cd ../project-hotfix
# Make changes...

# List worktrees
git worktree list

# Remove when done
git worktree remove ../project-hotfix
```

---

## Common Workflow Scenarios

### Scenario: You're Working on a Team

```bash
# Daily routine:
git checkout main
git pull
git checkout feature/my-task
git rebase main  # Keep your branch up to date
# Work...
git add .
git commit -m "Progress on my task"
git push
```

### Scenario: You're Contributing to Open Source

```bash
# Fork on GitHub, then:
git clone git@github.com:you/project.git
cd project
git remote add upstream git@github.com:original/project.git

# For each contribution:
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
git checkout -b feature/my-contribution
# Work...
git push -u origin feature/my-contribution
# Create PR on GitHub
```

### Scenario: Emergency Hotfix

```bash
# You're on a feature branch, but need to fix production
git stash save "WIP: feature work"
git checkout main
git pull
git checkout -b hotfix/urgent-fix
# Fix...
git add .
git commit -m "Fix urgent production issue"
git push -u origin hotfix/urgent-fix
# Create PR, get it merged ASAP
git checkout feature/my-feature
git stash pop
```

---

## Tips for Success

1. **Commit often**: Small, focused commits are easier to review and revert
2. **Pull before you start**: Always get the latest changes before beginning work
3. **Write good commit messages**: Future you will thank you
4. **Use branches liberally**: They're cheap and keep your work organized
5. **Keep your main branch clean**: Never commit directly to main
6. **Review your changes before committing**: Use `git diff` to catch mistakes
7. **Don't fear Git**: Most mistakes are fixable
8. **Learn the basics deeply**: Understanding how Git works makes everything easier

---

## Troubleshooting Guide

See the [command reference cheatsheet](git-github-cheatsheet.md) for specific command solutions to common problems!
