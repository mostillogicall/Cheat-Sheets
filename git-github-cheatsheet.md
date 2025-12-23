# Git & GitHub Cheat Sheet

*Your Quick Reference Guide*

---

## Initial Setup

### Configure Git

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --list    # View all settings
```

### SSH Key Setup (GitHub)

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
cat ~/.ssh/id_ed25519.pub    # Copy this to GitHub
```

Add the key at: Settings → SSH and GPG keys → New SSH key

---

## Repository Basics

### Creating Repositories

```bash
git init                     # Start new local repo
git clone <url>              # Clone existing repo
git clone <url> <dir>        # Clone to specific directory
```

### Basic Workflow

```bash
git status                   # Check repo status
git add <file>               # Stage specific file
git add .                    # Stage all changes
git commit -m "message"      # Commit staged changes
git commit -am "message"     # Add + commit (tracked files)
```

---

## Branching & Merging

### Branch Operations

```bash
git branch                   # List local branches
git branch -a                # List all branches
git branch <name>            # Create new branch
git checkout <branch>        # Switch branches
git checkout -b <branch>     # Create + switch
git switch <branch>          # Modern way to switch
git switch -c <branch>       # Modern create + switch
git branch -d <branch>       # Delete branch (safe)
git branch -D <branch>       # Force delete branch
```

### Merging

```bash
git merge <branch>           # Merge branch into current
git merge --no-ff <branch>   # Force merge commit
git merge --abort            # Cancel merge
```

---

## Working with Remotes

### Remote Management

```bash
git remote -v                # List remotes
git remote add origin <url>  # Add remote
git remote remove <name>     # Remove remote
git remote set-url origin <url>  # Change remote URL
```

### Syncing Changes

```bash
git fetch                    # Download remote changes
git fetch origin             # Fetch specific remote
git pull                     # Fetch + merge
git pull --rebase            # Fetch + rebase
git push                     # Push to remote
git push -u origin <branch>  # Push + track branch
git push --force             # Force push (careful!)
git push --delete origin <branch>  # Delete remote branch
```

---

## History & Inspection

```bash
git log                      # View commit history
git log --oneline            # Compact history
git log --graph --all        # Visual branch graph
git log -p                   # Show changes in commits
git log --author="Name"      # Filter by author
git show <commit>            # Show commit details
git diff                     # Changes not staged
git diff --staged            # Changes staged
git diff <branch1> <branch2> # Compare branches
git blame <file>             # Show who changed each line
```

---

## Undoing Changes

### Unstaging & Discarding

```bash
git restore <file>           # Discard working changes
git restore --staged <file>  # Unstage file
git checkout -- <file>       # Old way to discard
git reset HEAD <file>        # Old way to unstage
```

### Commits

```bash
git commit --amend           # Modify last commit
git reset --soft HEAD~1      # Undo commit, keep changes staged
git reset --mixed HEAD~1     # Undo commit, keep changes unstaged
git reset --hard HEAD~1      # Undo commit, discard changes
git revert <commit>          # Create new commit that undoes
```

---

## Stashing

```bash
git stash                    # Stash changes
git stash save "message"     # Stash with message
git stash list               # List stashes
git stash pop                # Apply + delete stash
git stash apply              # Apply stash (keep it)
git stash drop               # Delete stash
git stash clear              # Delete all stashes
```

---

## Advanced Operations

### Rebasing

```bash
git rebase <branch>          # Rebase onto branch
git rebase -i HEAD~3         # Interactive rebase last 3
git rebase --continue        # Continue after fixing
git rebase --abort           # Cancel rebase
```

### Cherry-picking

```bash
git cherry-pick <commit>     # Apply specific commit
git cherry-pick <commit1> <commit2>  # Multiple commits
```

### Tags

```bash
git tag                      # List tags
git tag v1.0.0               # Create lightweight tag
git tag -a v1.0.0 -m "msg"   # Create annotated tag
git push origin v1.0.0       # Push tag
git push origin --tags       # Push all tags
```

---

## GitHub-Specific Features

### Pull Requests

- Fork repo → Clone your fork → Create branch → Make changes → Push
- On GitHub: New Pull Request → Select branches → Create PR
- Review → Address feedback → Merge when approved

### Forking Workflow

```bash
# After forking on GitHub:
git clone <your-fork-url>
git remote add upstream <original-repo-url>
git fetch upstream
git merge upstream/main      # Sync with original
```

### GitHub CLI (gh)

```bash
gh auth login                # Authenticate
gh repo create               # Create repo
gh pr create                 # Create PR
gh pr list                   # List PRs
gh issue create              # Create issue
```

---

## .gitignore Essentials

Common patterns to exclude from version control:

```gitignore
# Dependencies
node_modules/
vendor/

# Environment
.env
.env.local

# Build outputs
dist/
build/
*.pyc

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db
```

---

## Common Troubleshooting

| Problem | Solution |
|---------|----------|
| Made changes to wrong branch | `git stash` → `git checkout correct-branch` → `git stash pop` |
| Merge conflict | 1. Edit conflicted files<br>2. Remove conflict markers<br>3. `git add <files>`<br>4. `git commit` |
| Pushed wrong commit | `git revert <commit>` → `git push` (Creates new commit to undo) |
| Accidentally deleted file | `git restore <file>` or `git checkout HEAD <file>` |
| Need to undo local commits | `git reset --soft HEAD~n` (Replace n with number of commits) |
| Detached HEAD state | `git checkout <branch-name>` or `git switch <branch-name>` |

---

## Best Practices

### Commit Messages

- Use present tense: "Add feature" not "Added feature"
- Keep first line under 50 characters
- Add detailed description after blank line if needed
- Reference issues: "Fix bug (#123)"

### Branch Naming

- `feature/add-login`
- `bugfix/fix-header`
- `hotfix/critical-security-patch`
- `release/v1.0.0`

### General Tips

- Commit early and often
- Pull before you push
- Never commit secrets or sensitive data
- Use .gitignore liberally
- Review changes before committing
- Keep branches focused and short-lived
- Don't rewrite public history (avoid force push to shared branches)

---

## Useful Resources

- Official Git Documentation: [git-scm.com/doc](https://git-scm.com/doc)
- GitHub Docs: [docs.github.com](https://docs.github.com)
- Interactive Git Branching: [learngitbranching.js.org](https://learngitbranching.js.org)
- Git Cheat Sheet: [education.github.com/git-cheat-sheet-education.pdf](https://education.github.com/git-cheat-sheet-education.pdf)
- Oh Shit, Git!: [ohshitgit.com](https://ohshitgit.com) (troubleshooting)
