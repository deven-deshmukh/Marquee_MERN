# Git Learning: Interactive Examples & Q&A

## 📌 Your Current Situation

Your repository is ready! You have:
- ✅ Git initialized
- ✅ Connected to GitHub
- ✅ Existing commits

Current status shows:
- Modified: `README.md`
- Untracked: `index.html`

---

## 🎓 Let's Learn by Doing!

### Example 1: Your First Commit

**What's happening now:**
```
Your computer (Working Directory)
    ├── README.md (modified)
    └── index.html (new file)
```

**What to do:**

Step 1 - Check what changed:
```bash
git status
```

Output:
```
On branch main
Changes not staged for commit:
  modified:   README.md

Untracked files:
  index.html
```

**What this means:**
- README.md was changed but not staged
- index.html is brand new and not tracked

---

Step 2 - See the actual changes:
```bash
git diff README.md
```

This shows exactly what changed (lines with `-` are deleted, `+` are added)

---

Step 3 - Stage all changes:
```bash
git add .
```

The `.` means "all files in current directory"

---

Step 4 - Verify staging:
```bash
git status
```

Output should show:
```
Changes to be committed:
  modified:   README.md
  new file:   index.html
```

---

Step 5 - Create the commit:
```bash
git commit -m "feat: add HTML page and update README"
```

✅ **You've created your first snapshot!**

---

Step 6 - View what you created:
```bash
git log --oneline -2
```

Output:
```
abc1234 (HEAD -> main) feat: add HTML page and update README
def5678 Update ReadMe.md
```

---

### Example 2: Working with Branches

**Why branches?** Think of them as parallel universes:

```
Universe 1 (main):      A---B---D---E
                         \
Universe 2 (feature):     C---C'
```

**Create a new feature branch:**

```bash
git branch feature/improve-styling
git checkout feature/improve-styling
# OR shorter: git checkout -b feature/improve-styling
```

Now you're on the feature branch. Files look the same, but changes won't affect main.

**Make changes on the branch:**

Edit `styles.css` (or create it):
```css
body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 20px;
}
```

```bash
git add styles.css
git commit -m "feat: add beautiful styling"
```

**Switch back to main (without your styling!):**
```bash
git checkout main
```

Notice: `styles.css` disappears because it only exists on the feature branch!

**Merge feature into main:**
```bash
git merge feature/improve-styling
```

Now `styles.css` appears on main! 

**View the history:**
```bash
git log --oneline --graph -5
```

Output shows the branch merge:
```
*   1a2b3c (HEAD -> main) Merge branch 'feature/improve-styling'
|\
| * 4d5e6f (feature/improve-styling) feat: add beautiful styling
|/
* 7g8h9i feat: add HTML page and update README
```

---

### Example 3: Handling Mistakes

**Scenario: You made a mistake!**

```bash
# You accidentally edited wrong file
# Quick fix - restore it
git restore filename.py
```

**Scenario: You staged wrong file:**

```bash
# Oops, staged something by mistake
git restore --staged wrong_file.py

# Now it's back to "modified" state (unstaged)
# You can edit it more or restore it completely
```

**Scenario: You committed wrong thing:**

```bash
# Last commit was a mistake
git reset --soft HEAD~1
```

This:
- ✅ Undoes the commit
- ✅ Keeps your changes (so you can fix and recommit)
- ❌ Doesn't delete anything

```bash
# Now fix the problem
git add correct_files.py
git commit -m "feat: correct version"
```

---

### Example 4: Real AIML Workflow

**Scenario: You're training multiple models**

Branch structure:
```bash
# Branch for baseline model
git checkout -b model/baseline
# ... modify, train, commit

# Branch for improved model
git checkout -b model/v2-with-regularization
# ... modify, train, commit

# Branch for experimental approach
git checkout -b experiment/gan-based
# ... try new approach, commit

# Check all branches
git branch
# Output:
# experiment/gan-based
# main
# model/baseline
# model/v2-with-regularization
#* <-- current branch
```

**Compare models' code:**
```bash
git diff model/baseline model/v2-with-regularization
```

**Best model won? Merge it:**
```bash
git checkout main
git merge model/v2-with-regularization
git branch -d model/baseline              # Clean up old branches
```

**Tag this version:**
```bash
git tag v1.0-baseline
git tag v1.1-with-regularization          # New best!
git tag -a v1.1-with-regularization -m "92% accuracy on test set"
```

---

## ❓ FAQ - Common Questions

### Q1: "I forgot to add a file to my last commit!"

**Answer:**
```bash
# Add the forgotten file
git add forgotten_file.py

# Amend the last commit
git commit --amend

# Your last commit now includes the file!
```

### Q2: "What's the difference between `git restore` and `git reset`?"

**Answer:**

| Command | What it does | When to use |
|---------|-------------|------------|
| `git restore` | Undo changes in working directory | Oops, edited wrong thing |
| `git restore --staged` | Unstage files | Oops, staged wrong file |
| `git reset --soft HEAD~1` | Undo commit, keep changes | Committed too early, want to fix |

### Q3: "Can I recover deleted commits?"

**Answer: YES!** Use `git reflog`:

```bash
# See ALL recent commits (even deleted ones!)
git reflog

# Output shows:
# abc1234 HEAD@{0}: commit: fix: something
# def5678 HEAD@{1}: reset: back to abc1234
# ghi9012 HEAD@{2}: commit: feat: something

# Recover the commit!
git checkout abc1234   # Go to that point
# OR
git branch recover-branch abc1234  # Create new branch at that commit
```

### Q4: "How do I fix a merge conflict?"

**Answer:**

When merging, you might see:
```
Auto-merging model.py
CONFLICT (content conflict): merge conflict in model.py
Automatic merge failed; fix conflicts and then commit the result.
```

Open `model.py` and look for:
```python
<<<<<<< HEAD
# Your code
=======
# Their code
>>>>>>> feature/branch
```

**Fix it:** Decide which code to keep (or combine both), delete the markers:
```python
# Keep both versions combined
# Your code + Their code combined properly
```

Then:
```bash
git add model.py
git commit -m "fix: resolve merge conflict"
```

### Q5: "Should I use `git rebase`?"

**Answer:** NOT yet! For now, stick with `git merge`. Rebase is an advanced technique for keeping history clean. Once you're comfortable with basics, explore it!

### Q6: "How do I undo a push to GitHub?"

**Answer:** You have options (ordered by safety):

```bash
# Safest: Revert creates a new commit undoing changes
git revert HEAD
git push

# Careful: Reset locally (don't do this on shared branches!)
git reset --soft HEAD~1
git commit -m "fixed version"
git push --force-with-lease
```

### Q7: "Why does my main branch show origin/main in log?"

**Answer:** Because your branch is tracking the remote!

```bash
# Local main branch
# ↓
# origin/main is the remote tracking branch (your copy of GitHub's version)
# ↓
# The actual remote on GitHub servers
```

Keep them synced with:
```bash
git pull                # Fetch + merge
git push                # Send your commits
```

---

## 🎯 Mini Challenges

Try these to practice:

### Challenge 1: Create 3 commits with good messages
```bash
# Make 3 separate, logical changes
# Each with a meaningful commit message
# Then: git log --oneline -3
```

### Challenge 2: Create and merge a branch
```bash
# Create feature/add-comments
# Make a change, commit
# Switch to main
# Merge feature/add-comments
# View the graph: git log --graph --oneline -5
```

### Challenge 3: Practice undoing
```bash
# Edit a file by mistake
# Use git restore to undo it
# Verify it's restored: cat filename
```

### Challenge 4: Create meaningful tags
```bash
git tag v0.1-initial-setup
git tag v0.2-added-styling
git tag -a v0.3-improved -m "Performance improvements"
git tag -l                  # View all
```

---

## 🚀 When You're Ready: GitHub

Once comfortable with local Git, try:

```bash
# Push to GitHub
git push origin main

# Pull team's work
git pull origin main

# Create feature branch on GitHub
git push origin feature/new-model

# Create Pull Request (on GitHub website)
# - Go to your fork
# - Click "New Pull Request"
# - Team reviews your code
# - Merge when approved!
```

---

## 💡 Key Insights for AIML

1. **Version everything**: Not just code, but model checkpoints, data versions
2. **Document experiments**: Commit messages should explain what you tried
3. **Use branches for hypotheses**: Each model variation gets a branch
4. **Tag releases**: `v1.0-baseline`, `v1.1-with-augmentation`
5. **Share results**: Include accuracy/loss in commit messages

Example AIML workflow:
```bash
# Try batch normalization
git checkout -b exp/batch-norm
# ... code changes, training, validation
git commit -m "exp: batch normalization - 91.2% accuracy"

# Try dropout instead
git checkout main
git checkout -b exp/dropout
# ... code changes, training
git commit -m "exp: dropout (p=0.5) - 90.8% accuracy"

# Batch norm won! Merge it
git merge exp/batch-norm
git tag v1.1-with-batch-norm
```

---

## 🎓 Next Steps

1. ✅ **Read**: The guides above
2. ⏭️ **Practice**: Do the exercises in `HANDS_ON_EXERCISES.md`
3. 🔄 **Repeat**: Create commits until it's natural
4. 👥 **Collaborate**: Push to GitHub and make Pull Requests
5. 🌟 **Master**: Learn Git rebase, cherry-pick, and advanced flows

---

**Remember**: Git has a learning curve, but it's worth it! Every software engineer uses it. You're building a crucial skill! 🎉
