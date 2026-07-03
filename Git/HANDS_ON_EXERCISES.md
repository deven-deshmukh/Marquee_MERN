# Hands-On Git Practice Exercises

## Exercise 1: Basic Git Workflow (Start Here! 👈)

### Objective
Learn the fundamental commit cycle: modify → stage → commit

### Steps

**Step 1: Check Git Status**
```bash
cd FirstDemo
git status
```
Expected output: Shows branch and any changes

**Step 2: Modify README**
- Open [README.md](README.md)
- Add this line at the end:
```markdown

## Getting Started with Git
This project is a learning repository for Git and GitHub!
```

**Step 3: Check Changes**
```bash
git status                 # See what changed
git diff README.md        # See exact changes
```

**Step 4: Stage Changes**
```bash
git add README.md
git status                # Notice it says "Changes to be committed"
```

**Step 5: Commit**
```bash
git commit -m "docs: add getting started section to README"
```

**Step 6: View History**
```bash
git log --oneline -3
```

**What You Learned:**
✅ Working Directory → Staging Area → Repository
✅ git status, git diff, git add, git commit
✅ Viewing commit history

---

## Exercise 2: Creating and Merging Branches

### Objective
Learn how to isolate work using branches

### Steps

**Step 1: View Branches**
```bash
git branch
```

**Step 2: Create Feature Branch**
```bash
git checkout -b feature/add-styles
```
Alternative (modern syntax):
```bash
git switch -c feature/add-styles
```

**Step 3: Create New File**
Create `styles.css`:
```css
body {
    font-family: Arial, sans-serif;
    margin: 20px;
    background-color: #f5f5f5;
}

p {
    color: #333;
    line-height: 1.6;
}
```

**Step 4: Commit on Feature Branch**
```bash
git add styles.css
git commit -m "feat: add basic CSS styling"
```

**Step 5: Switch Back to Main**
```bash
git checkout main
# or: git switch main
```
Notice: `styles.css` disappears! (It exists only on feature branch)

**Step 6: Merge Feature Into Main**
```bash
git merge feature/add-styles
```
Notice: `styles.css` now appears!

**Step 7: View Merged History**
```bash
git log --oneline --graph -5
```

**Step 8: Delete Feature Branch**
```bash
git branch -d feature/add-styles
```

**What You Learned:**
✅ Creating branches (git checkout -b)
✅ Switching branches (git switch)
✅ Merging branches (git merge)
✅ Branches are isolated development lines
✅ Deleting branches (git branch -d)

---

## Exercise 3: Understanding Commits

### Objective
Learn how commits work and create meaningful commit history

### Steps

**Step 1: Create Multiple Small Commits**

Create file `index.html.bak`:
```html
<!-- Updated version -->
<html>
  <head>
    <title>Demo Page</title>
    <link rel="stylesheet" href="styles.css">
  </head>
  <body>
    <h1>Welcome!</h1>
    <p>Hello World</p>
  </body>
</html>
```

**Step 2: Make Commit 1**
```bash
git add index.html.bak
git commit -m "feat: create enhanced HTML structure"
```

**Step 3: Make Another Change**
Edit `index.html` - replace content with:
```html
<!-- Updated -->
<p>Hello World - Updated</p>
```

**Step 4: Make Commit 2**
```bash
git add index.html
git commit -m "fix: update greeting message"
```

**Step 5: Examine Commits**
```bash
# View last 3 commits
git log --oneline -3

# See details of a specific commit
git log -1 -p

# See what changed between commits
git diff HEAD~2 HEAD
```

**Step 6: View Changes in Each Commit**
```bash
# Show last commit details
git show HEAD

# Show previous commit
git show HEAD~1
```

**What You Learned:**
✅ Each commit is a snapshot
✅ View commit details (git show)
✅ Compare between commits (git diff)
✅ Understand commit messages matter for clarity

---

## Exercise 4: Undoing Changes (Safety!)

### Objective
Learn how to undo mistakes safely

### Steps

**Step 1: Make an Accidental Change**
Edit `README.md` and add garbage:
```
This is a mistake - delete this!
asdfghjkl
```

**Step 2: Undo Unstaged Changes**
```bash
git status              # See the modified file
git diff README.md      # See the bad changes
git restore README.md   # Undo changes
cat README.md           # Verify it's restored
```

**Step 3: Make Changes but Regret Staging**
Edit `index.html`:
```html
<p>This change is wrong</p>
```

**Step 4: Stage Then Unstage**
```bash
git add index.html
git status              # See "Changes to be committed"
git restore --staged index.html    # Unstage
git status              # Changes still in working directory
git restore index.html  # Now undo the changes
```

**Step 5: Commit Wrong Thing, Then Fix**
```bash
# Make a bad commit
echo "bad content" >> index.html
git add .
git commit -m "bad commit"

# Option 1: Undo the commit but keep changes
git reset --soft HEAD~1
git restore --staged .
git restore .

# Option 2: Revert (safe for already-pushed commits)
# git revert HEAD  # Creates new commit that undoes previous
```

**What You Learned:**
✅ git restore - undo unstaged changes
✅ git restore --staged - unstage files
✅ git reset --soft - undo commit, keep changes
✅ git revert - safely undo published commits

---

## Exercise 5: Working with Tags

### Objective
Learn to mark important points (like released versions)

### Steps

**Step 1: Create a Tag for Current Commit**
```bash
git tag v1.0.0
```

**Step 2: Add Annotation to Tag**
```bash
git tag -a v1.0.1 -m "First stable version"
```

**Step 3: View Tags**
```bash
git tag
git tag -l -n1          # Show tags with messages
```

**Step 4: Create Another Commit and Tag It**
```bash
# Make a change
echo "<!-- version 1.1 -->" >> index.html
git add .
git commit -m "chore: bump version"

# Tag this commit
git tag v1.1.0
```

**Step 5: View Commits with Tags**
```bash
git log --oneline --decorate -5
```

**What You Learned:**
✅ Tags mark important commits
✅ Useful for version tracking
✅ Different from branches (fixed points vs. moving lines)

---

## Exercise 6: Viewing Project History

### Objective
Master different ways to view project history

### Steps

**Step 1: Simple Log**
```bash
git log --oneline -10           # Last 10 commits, one per line
git log -5                      # Last 5 commits, detailed
git log --oneline --graph -10   # With branch visualization
```

**Step 2: Filter by Author**
```bash
git log --author="Deven"        # Your commits
```

**Step 3: Filter by Date**
```bash
git log --since="2 days ago"
git log --until="1 week ago"
```

**Step 4: Search Commit Messages**
```bash
git log --grep="feature"        # Find commits mentioning "feature"
```

**Step 5: View Statistics**
```bash
git log --stat -3               # Last 3 commits with stats
git log --stat --oneline -3     # Compact version
```

**Step 6: Advanced Visualization**
```bash
git log --oneline --graph --all --decorate
```

**What You Learned:**
✅ Multiple ways to view history
✅ Filter commits by various criteria
✅ Understand project evolution

---

## Summary Checklist

After completing these exercises, you should understand:

- [ ] **Exercise 1**: Basic workflow (status → add → commit)
- [ ] **Exercise 2**: Branches and merging
- [ ] **Exercise 3**: Commits as snapshots
- [ ] **Exercise 4**: Undoing changes safely
- [ ] **Exercise 5**: Tags for versions
- [ ] **Exercise 6**: Viewing and analyzing history

---

## Practice Challenges

Try these on your own:

1. **Challenge 1**: Create 3 feature branches, make different changes on each, then merge them to main
2. **Challenge 2**: Create a branch, make a mistake, undo it using git restore
3. **Challenge 3**: Create commits with meaningful messages, view the log
4. **Challenge 4**: Create tags for different versions
5. **Challenge 5**: View the git log in 3 different formats

---

## Common Gotchas

| Mistake | Solution |
|---------|----------|
| Changed file but can't see it | `git add .` first |
| Lost changes | `git reflog` to find them |
| Committed on wrong branch | `git reset --soft HEAD~1`, switch branch, commit again |
| Can't switch branch | Commit or stash changes first: `git stash` |
| Deleted branch by accident | `git reflog` and `git branch <name> <commit-hash>` |

---

## Next: GitHub Integration

Ready to push to GitHub? See the main guide for:
- Setting up remote repository
- Pushing and pulling
- Creating Pull Requests
- Collaborating with others

Good luck! 🎉
