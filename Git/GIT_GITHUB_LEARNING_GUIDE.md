# Git & GitHub Learning Guide for AIML Students 🚀

## Table of Contents
1. [What is Git & GitHub?](#what-is-git--github)
2. [Core Concepts](#core-concepts)
3. [Basic Commands](#basic-commands)
4. [Workflow Examples](#workflow-examples)
5. [GitHub Integration](#github-integration)
6. [Best Practices for AIML Projects](#best-practices-for-aiml-projects)
7. [Common Scenarios](#common-scenarios)

---

## What is Git & GitHub?

### Git
- **Distributed Version Control System**: Tracks changes in your code over time
- **Local**: Works on your computer without needing internet
- **Snapshots**: Stores complete project snapshots at different points

### GitHub
- **Cloud Platform**: Hosts your Git repositories online
- **Collaboration**: Multiple people can work on the same project
- **Social Coding**: Share code, contribute to open-source
- **CI/CD Integration**: Automate testing and deployment

**Why for AIML?**
- Track model versions and experiment changes
- Collaborate with team members on datasets and code
- Reproduce results with specific commits
- Build your portfolio

---

## Core Concepts

### 1. **Repository (Repo)**
```
Your project folder with all files and Git history
├── .git/           (Git metadata - auto-created)
├── src/
├── models/
├── data/
└── README.md
```

### 2. **The Three Stages**
```
Working Directory → Staging Area → Repository
    (your files)     (git add)    (git commit)
```

### 3. **Commits**
- Snapshot of your code at a point in time
- Each has a unique ID (hash)
- Contains: what changed, who changed it, when, why

### 4. **Branches**
- Independent lines of development
- Main/Master branch: production code
- Feature branches: work on features separately
```
master:  A---B---D---E
           \
feature:    C---C'
```

### 5. **Merging**
- Combine changes from different branches
- Brings feature back to master

---

## Basic Commands

### Setup
```bash
# Configure your identity (one-time)
git config --global user.name "Your Name"
git config --global user.email "your.email@gmail.com"

# Verify
git config --list | grep user
```

### Starting a Repository
```bash
# Create new repo
git init

# Clone existing repo from GitHub
git clone https://github.com/username/repo.git
```

### Checking Status
```bash
# See what changed
git status

# See detailed changes
git diff                 # Changes in working directory
git diff --staged        # Changes in staging area
git diff HEAD~1 HEAD     # Compare last 2 commits
```

### Tracking Changes
```bash
# Add specific file
git add filename.py

# Add all changes
git add .

# Unstage file
git restore --staged filename.py
```

### Committing
```bash
# Create snapshot with message
git commit -m "Add data preprocessing module"

# Add and commit together
git commit -am "Fix bug in model training"

# Amend last commit (if not pushed yet!)
git commit --amend
```

### Viewing History
```bash
# One-line history
git log --oneline

# With branch visualization
git log --oneline --graph --all

# By author
git log --author="Your Name"

# Last 5 commits
git log -5

# Show specific commit
git show abc123def
```

### Branches
```bash
# List branches
git branch
git branch -a            # Including remote branches

# Create new branch
git branch feature/neural-network

# Switch branch
git checkout feature/neural-network
git switch feature/neural-network    # Modern syntax

# Create and switch in one command
git checkout -b feature/data-augmentation

# Delete branch
git branch -d feature/old-feature
git branch -D feature/old-feature    # Force delete
```

### Merging
```bash
# Switch to master first
git checkout master

# Merge feature into master
git merge feature/neural-network

# Merge with commit message
git merge feature/data-augmentation -m "Add data augmentation"
```

### Undoing Changes
```bash
# Undo changes in working directory
git restore filename.py

# Undo staging
git restore --staged filename.py

# Go back to previous commit (create new commit)
git revert abc123def

# Reset to previous commit (⚠️ loses changes)
git reset --hard abc123def

# Check what was lost
git reflog
```

---

## Workflow Examples

### Example 1: Simple Linear Workflow
```bash
# Initialize repo
cd my-ml-project
git init

# Create your first file
echo "# ML Project" > README.md

# Track and commit
git add .
git commit -m "Initial commit"

# Make changes
# (edit files...)

git add modified_file.py
git commit -m "Add feature X"

# View history
git log --oneline
```

### Example 2: Feature Branch Workflow (Recommended for Teams)
```bash
# Start from main branch
git checkout main
git pull origin main        # Get latest

# Create feature branch
git checkout -b feature/model-improvement

# Make changes and commit
git add .
git commit -m "Improve model accuracy with regularization"

# Push to GitHub
git push origin feature/model-improvement

# Create Pull Request on GitHub (web interface)
# Team reviews → merge to main
```

### Example 3: Working with Remote (GitHub)
```bash
# Add remote
git remote add origin https://github.com/username/repo.git

# Push local branch to GitHub
git push -u origin main

# Pull latest changes
git pull origin main

# View remotes
git remote -v

# Remove remote
git remote remove origin
```

---

## GitHub Integration

### Setting Up SSH Keys (Recommended)
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@gmail.com"

# Add to SSH agent
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519

# Copy public key to GitHub
cat ~/.ssh/id_ed25519.pub

# GitHub Settings → SSH and GPG keys → New SSH key → Paste
```

### Create Repository on GitHub
1. Go to github.com/new
2. Name it: `my-aiml-project`
3. Add description
4. Choose Public (for portfolio) or Private
5. Skip template
6. Click "Create repository"

### Connect Local Repo to GitHub
```bash
git remote add origin https://github.com/YOUR_USERNAME/my-aiml-project.git
git branch -M main
git push -u origin main
```

### Fork & Contribute to Others' Projects
```bash
# Click "Fork" on GitHub (creates copy in your account)

# Clone your fork
git clone https://github.com/YOUR_USERNAME/original-repo.git

# Add upstream to track original
git remote add upstream https://github.com/ORIGINAL_OWNER/original-repo.git

# Keep fork updated
git fetch upstream
git rebase upstream/main

# Push changes to your fork
git push origin feature/improvement

# Create Pull Request on GitHub
```

---

## Best Practices for AIML Projects

### Directory Structure
```
ml-project/
├── .gitignore              # Exclude large files
├── README.md               # Project description
├── requirements.txt        # Python dependencies
├── data/                   # Only small CSVs
│   └── sample.csv
├── models/                 # Trained models (⚠️ exclude via .gitignore)
├── src/
│   ├── preprocessing.py
│   ├── model.py
│   └── train.py
├── notebooks/
│   └── exploration.ipynb
└── tests/
    └── test_model.py
```

### .gitignore for AIML
```
# Python
__pycache__/
*.pyc
*.pyo
*.egg-info/
.env

# Jupyter
.ipynb_checkpoints/

# Data & Models (Large files)
*.csv
*.parquet
models/*.pkl
models/*.h5
data/raw/
data/processed/

# IDE
.vscode/
.idea/
*.swp

# System
.DS_Store
Thumbs.db
```

### Commit Message Convention
```
<type>(<scope>): <subject>

<body>

<footer>

Examples:
feat(model): add LSTM layer for sequence processing
fix(preprocessing): handle missing values correctly
docs(readme): add installation instructions
test(model): add unit tests for preprocessing
refactor(train): optimize batch processing
```

### Version Your Models
```bash
# Tag commits for model versions
git tag -a v1.0.0 -m "Initial model - 92% accuracy"
git push origin v1.0.0

# View tags
git tag -l
```

---

## Common Scenarios

### Scenario 1: Made Changes, Want to Undo
```bash
# Option 1: Discard all changes
git restore .

# Option 2: Create backup first
git branch backup/my-changes
git restore .

# Option 3: If committed
git revert abc123def    # Safe - creates new commit
```

### Scenario 2: Committed Wrong Thing
```bash
# Not pushed yet?
git reset --soft HEAD~1    # Undo commit, keep changes
git restore --staged .      # Unstage
# Now recommit correctly

# Already pushed?
git revert abc123def       # Create new commit undoing changes
git push origin main
```

### Scenario 3: Merge Conflict
```bash
# During merge, you get conflict message
# Conflicts appear in files like:
# <<<<<<< HEAD
# Your changes
# =======
# Their changes
# >>>>>>> feature-branch

# Fix conflicts manually in editor
git add conflicted_file.py

# Complete merge
git commit -m "Resolve merge conflict"
```

### Scenario 4: Switching Between Models/Experiments
```bash
# Create branch for each experiment
git checkout -b exp/batch-norm-v1
# Train and modify model...
git commit -am "Test batch normalization"

# Switch to another experiment
git checkout exp/dropout-v1
# Training uses different code

# Merge best experiment to main
git checkout main
git merge exp/batch-norm-v1
```

### Scenario 5: Collaborate with Team
```bash
# You and teammate both working on same feature
git pull origin main                # Get latest
git checkout -b feature/preprocessing

# You make changes
git add .
git commit -m "Add data cleaning"
git push origin feature/preprocessing

# Teammate makes changes on same branch
git pull origin feature/preprocessing    # Get their work

# Resolve any conflicts
# Continue developing
git push origin feature/preprocessing
```

---

## Key Takeaways for AIML Students

✅ **DO:**
- Commit frequently (after each logical change)
- Write clear commit messages (helps future you!)
- Use branches for experiments
- Create a `requirements.txt`: `pip freeze > requirements.txt`
- Add detailed README (usage, results, citations)
- Tag important model versions
- Use .gitignore for large files

❌ **DON'T:**
- Commit large datasets (> 100MB)
- Commit trained models (> 50MB)
- Commit credentials or API keys
- Force push to shared branches (`git push -f`)
- Mix multiple features in one commit
- Ignore merge conflicts

---

## Next Steps

1. **Practice**: Create a sample AIML project
2. **Experiment**: Use branches for different models
3. **Collaborate**: Fork a project, make improvements, create PR
4. **Share**: Make your projects public on GitHub
5. **Learn**: Check git documentation: `git help <command>`

---

## Quick Reference

```bash
# Essential commands
git status              # Check status
git add .              # Stage changes
git commit -m "msg"    # Commit
git push               # Push to GitHub
git pull               # Get latest
git log --oneline      # View history
git branch -a          # View branches
git checkout -b name   # Create branch
git merge branch       # Merge branch
```

Happy coding! 🎉
