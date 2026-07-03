# Git Command Quick Reference Card

## 🚀 Most Used Commands (90% of the time)

```bash
# See current state
git status

# Add changes to staging area
git add filename.py
git add .                    # Add all changes

# Create a snapshot
git commit -m "Your message"

# Send to GitHub
git push

# Get latest from GitHub
git pull

# See history
git log --oneline -5
```

---

## 📊 Status Check Flow

```
Working Directory (files you edit)
         ↓ (git add)
Staging Area (files ready to commit)
         ↓ (git commit)
Repository (your local .git folder)
         ↓ (git push)
GitHub Remote (the cloud copy)
```

---

## 🌿 Branch Commands

| Command | Purpose |
|---------|---------|
| `git branch` | List all branches |
| `git branch myfeature` | Create new branch |
| `git checkout myfeature` | Switch to branch |
| `git checkout -b myfeature` | Create AND switch |
| `git merge myfeature` | Merge into current branch |
| `git branch -d myfeature` | Delete branch |

---

## 🔄 Working with Remote (GitHub)

```bash
git remote -v                          # See remotes
git remote add origin <url>            # Add remote
git push origin main                   # Push to GitHub
git pull origin main                   # Pull from GitHub
git push origin myfeature              # Push feature branch
```

---

## ⏮️ Undoing Changes

| Situation | Command |
|-----------|---------|
| Edited file, not staged | `git restore filename` |
| Staged file, not committed | `git restore --staged filename` |
| Committed, want to undo | `git revert HEAD` (safe) |
| Committed, want to rewrite | `git reset --soft HEAD~1` |
| Want to see what was lost | `git reflog` |

---

## 📝 Good Commit Messages

❌ Bad:
```
git commit -m "fixed stuff"
git commit -m "asdf"
git commit -m "update"
```

✅ Good:
```
git commit -m "feat: add data preprocessing module"
git commit -m "fix: handle missing values in dataset"
git commit -m "docs: update README with examples"
git commit -m "test: add unit tests for model.py"
git commit -m "refactor: simplify feature extraction"
```

**Format**: `<type>: <description>`
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `test`: Tests
- `refactor`: Code improvement
- `perf`: Performance improvement
- `chore`: Maintenance

---

## 🔍 Viewing History

```bash
git log                              # Full details
git log --oneline                    # Concise
git log --oneline -5                 # Last 5 commits
git log --oneline --graph -10        # Visual graph
git log --author="Your Name"         # By author
git show HEAD                        # Latest commit details
git diff HEAD~1 HEAD                 # Compare last 2 commits
```

---

## 🏷️ Tags (Version Markers)

```bash
git tag v1.0.0                       # Create lightweight tag
git tag -a v1.0.0 -m "Version 1.0"  # Annotated tag
git tag                              # List tags
git push origin v1.0.0               # Push tag to GitHub
```

---

## 🚨 Emergency Commands

```bash
# "Oh no, I committed the wrong thing!"
git reset --soft HEAD~1

# "I made a mistake in my last commit message"
git commit --amend -m "New message"

# "I lost commits!"
git reflog                           # Find the commit hash
git checkout <hash>                  # Go back to it

# "I have uncommitted changes and need to switch branch"
git stash                            # Save changes temporarily
git checkout otherbranch
git stash pop                        # Restore changes

# "I want to see what I'm about to push"
git log origin/main..HEAD
```

---

## 💾 .gitignore Template for AIML

Create file named `.gitignore`:

```
# Python
__pycache__/
*.pyc
*.egg-info/
.env
venv/

# Jupyter
.ipynb_checkpoints/

# Large files
*.csv
*.pkl
*.h5
models/trained_models/
data/raw/

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db
```

---

## 📚 Getting Help

```bash
git help                             # General help
git help <command>                   # Help for specific command
git help commit                      # Example: help on commit
```

---

## 🎯 Your First Contribution Steps

1. **Clone a repo**: `git clone https://github.com/user/repo.git`
2. **Create branch**: `git checkout -b feature/my-change`
3. **Make changes**: Edit files
4. **Stage changes**: `git add .`
5. **Commit**: `git commit -m "feat: description"`
6. **Push**: `git push origin feature/my-change`
7. **Create PR**: Go to GitHub website, create Pull Request
8. **Wait for review**: Team reviews your changes
9. **Merge**: Approved → merge to main

---

## 🌟 Pro Tips

✅ Commit often (every logical chunk of work)
✅ Write clear messages (your future self will thank you)
✅ Use branches (keeps main stable)
✅ Pull before you push (avoid conflicts)
✅ Review before committing (`git diff`)

❌ Don't commit large files (use .gitignore)
❌ Don't force push to shared branches (`git push -f`)
❌ Don't commit secrets (API keys, passwords)
❌ Don't commit broken code to main

---

## 🎓 Learning Resources

- **Official Git**: https://git-scm.com/doc
- **GitHub Guides**: https://guides.github.com
- **Interactive Learning**: https://learngitbranching.js.org
- **Git Cheatsheet**: https://gitcheatsheet.com

---

**Remember**: Everyone makes Git mistakes! That's how we learn. 🎉
