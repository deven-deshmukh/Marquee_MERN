# Visual Git Concepts Guide 🎨

## 1. The Git Workflow (Your Daily Process)

```
┌─────────────────────────────────────────────────┐
│           YOUR COMPUTER                         │
│                                                 │
│  ┌──────────────┐     ┌──────────────┐        │
│  │  Working     │     │  Staging     │        │
│  │  Directory   │────>│  Area        │        │
│  │              │ add │              │        │
│  │ Your files   │     │ Ready to     │        │
│  │ (you edit)   │     │ commit       │        │
│  └──────────────┘     └──────────────┘        │
│                             │                  │
│                             │ commit           │
│                             ↓                  │
│                      ┌──────────────┐         │
│                      │  Repository  │         │
│                      │  (in .git)   │         │
│                      │              │         │
│                      │ Your commits │         │
│                      └──────────────┘         │
└─────────────────────────────────────────────────┘
            Your Git workflow cycle
```

**What happens at each step:**

### Step 1: Edit Files (Working Directory)
```bash
# You edit README.md
# You create new model.py
# Git notices changes
```

### Step 2: Stage Changes (Staging Area)
```bash
# You decide which changes to include
git add README.md          # Include this
git add model.py           # Include this
# Don't add config.py yet  # Exclude this for now
```

### Step 3: Commit (Repository)
```bash
# You create a snapshot
git commit -m "Add data cleaning and model training"
# These changes are now part of history!
```

---

## 2. Commits Are Snapshots

```
Current State of Your Project:
────────────────────────────

📸 Commit A (Initial)
├── model.py (100 lines)
├── data.csv (1000 rows)
└── README.md (basic)

📸 Commit B (Added preprocessing)
├── model.py (100 lines)
├── preprocessing.py (NEW)
├── data.csv (1000 rows)
└── README.md (basic)

📸 Commit C (Improved model) ← Current
├── model.py (150 lines, improved)
├── preprocessing.py (80 lines)
├── data.csv (1000 rows)
└── README.md (basic)

Each commit is a complete snapshot!
You can go back to ANY commit anytime!
```

**Why snapshots matter:**
- Commit A: "Original code"
- Commit B: "Data preprocessing working"
- Commit C: "Model improved"

If Commit C has a bug, you can:
1. Review what changed from B to C
2. Go back to Commit B
3. Start over from there

---

## 3. Branches - Parallel Development

```
Timeline →

        Create Feature Branch
                ↓

main:  A───B───D───E────────→ (production, stable)
            \
feature:     C───C'───C''──→ (experiment, work in progress)
                ↑
            Developing new feature
            (doesn't affect main!)

                When ready:
                
main:  A───B───D───E───F──→ (F = merged feature)
            \           /
feature:     C───C'───C''
                        ↑
                   Now merged!
```

**Real example:**

```
main:     data-cleaning───final-model───(ready for production)
              ↑
              |
              +───exp-branch-1───(testing batch norm)
              |
              +───exp-branch-2───(testing dropout)

You can develop 3 different ideas independently!
Then merge the best one back to main.
```

---

## 4. The Git Graph (Commit History)

```bash
# Run this to see it:
git log --oneline --graph --all

# Example output:
*   1a2b3c (HEAD -> main) Merge branch 'feature/cleanup'
|\
| * 4d5e6f (feature/cleanup) Add data cleaning
| * 7g8h9i (feature/cleanup) Remove outliers
|/
* 0j1k2l (main) Initial commit
```

**What the symbols mean:**

```
*     = A commit
|     = A line of commits
|\    = One commit splits into two (branch point)
|/    = Two branches merge back together
→     = Time flows this direction
```

---

## 5. Git States (Where Is Your Code?)

```
┌─────────────────────────────────────────────┐
│         GIT SPACES & REFERENCES             │
└─────────────────────────────────────────────┘

LOCAL YOUR COMPUTER
────────────────────────────
  ↓ You edit files
  [Working Directory]
  ↓ git add
  [Staging Area]
  ↓ git commit
  [Local Repository / .git folder]
  ↓ git push
  
REMOTE (GitHub)
────────────────
  [GitHub Servers]
  ↑ git pull
  [Your Copy of GitHub = origin/main]
  
TERMINOLOGY:
- main = Your local branch
- origin/main = GitHub's version
- HEAD = Your current position
```

---

## 6. The Merge Process

```
BEFORE MERGE:

main:    A───B───D───E
             \
feature:      C───C'

AFTER: git merge feature

main:    A───B───D───E───F (merge commit)
             \           /
feature:      C───C'────/

WHAT HAPPENED:
- All commits from feature branch (C, C') are now in main
- A new merge commit (F) records that these branches came together
- Both branches now point to F
```

**Types of merges:**

### Fast-Forward Merge (Simple)
```
Before:
main:    A───B
             ↑
feature:     C───D
             ↑

After:
main:    A───B───C───D
                     ↑
feature:            (same)

Why? feature was ahead of main, so main just catches up
```

### 3-Way Merge (With merge commit)
```
Before:
main:    A───B───D───E
             \
feature:      C───C'

Both have work since they split!
Need to combine (3-way: B, E, C')

After:
main:    A───B───D───E───F
             \           /
feature:      C───C'────/

F = merge commit combining E and C'
```

---

## 7. Merge Conflicts (When Things Collide)

```
Both branches edited the SAME FILE differently:

Feature branch says:
───────────────────
def predict(x):
    return model.predict(x) * 2  ← multiplies by 2

Main branch says:
──────────────────
def predict(x):
    return model.predict(x) + 1  ← adds 1

CONFLICT! Git doesn't know which version to use!

When merging, Git shows:
───────────────────────
<<<<<<< HEAD
    return model.predict(x) + 1  ← from main
=======
    return model.predict(x) * 2  ← from feature
>>>>>>> feature/experiment

YOU MUST FIX IT:
Choose ONE or COMBINE both:
  return model.predict(x) * 2 + 1  ← combine both!
```

---

## 8. Remote Repositories (GitHub)

```
                        INTERNET
                           ↕
    YOUR COMPUTER          GITHUB
    ──────────────        ──────
    
    main branch      ←push→   origin/main
    (your commits)          (server copy)
    
    tracking branch
    origin/main
    (local copy of
     what's on
     GitHub)

WORKFLOW:
┌──────────────────────────────────────┐
│ 1. git pull                          │
│    Fetch changes from GitHub ↓       │
│    Update your local branches ↓      │
│                                      │
│ 2. Make changes locally              │
│    Edit files, commit                │
│                                      │
│ 3. git push                          │
│    Send your commits to GitHub ↑     │
└──────────────────────────────────────┘
```

---

## 9. Three Types of Git Objects

```
┌─────────────────────────────────────┐
│      THREE THINGS GIT STORES        │
└─────────────────────────────────────┘

1. BLOB (Binary Large Object)
   └─ A file's contents
   └─ Stores the actual code

2. TREE
   └─ A directory snapshot
   └─ "At this point, files were:"
      - model.py
      - data.csv
      - README.md

3. COMMIT
   └─ Ties everything together
   └─ Contains:
      - Who made the change (author)
      - When (timestamp)
      - What changed (tree)
      - Why (message)
      - Parent commit (history link)

EXAMPLE:

Commit abc123:
  Author: You
  Date: Feb 6, 2026
  Message: "Add model training"
  Tree: model.py, train.py, requirements.txt
  Parent: def456 (previous commit)

When you run: git show abc123
Git finds the commit, gets the tree, shows the blobs!
```

---

## 10. The Git File States

```
        Untracked Files
        (Git ignores them)
             ↓ git add
        Staged Files
        (Ready to commit)
             ↓ git commit
        Committed Files
        (Part of history)
             ↓ (you edit)
        Modified Files
             ↓ git add
        (Back to staged)

EXAMPLE:
───────

You create new_model.py
↓
new_model.py is UNTRACKED

git add new_model.py
↓
new_model.py is STAGED

git commit -m "add model"
↓
new_model.py is COMMITTED

You edit new_model.py
↓
new_model.py is MODIFIED

git add new_model.py
↓
new_model.py is STAGED again

And the cycle continues...
```

---

## 11. The Diff Concept

```
WHAT IS A DIFF?
A diff shows what changed between versions

File Version A:
───────────────
def train(data):
    model = Model()
    model.fit(data)
    return model

File Version B:
───────────────
def train(data):
    model = Model()
    model.compile()      ← NEW LINE
    model.fit(data)
    model.save()         ← NEW LINE
    return model

THE DIFF:
────────
def train(data):
    model = Model()
+   model.compile()         ← Added
    model.fit(data)
+   model.save()            ← Added
    return model

Green lines (with +) = added
Red lines (with -) = removed
No change = no symbol
```

---

## 12. Understanding HEAD

```
HEAD is your current location in Git!

main:  A───B───C───D
           ↑
           └─ HEAD points here (current position)

You can move HEAD:
        
main:  A───B───C───D
                   ↑
                   └─ After running: git checkout D
                      (HEAD moved here)

In commits:

git show HEAD      # Show current commit
git show HEAD~1    # Show parent commit
git show HEAD~2    # Show grandparent commit

In branches:

git checkout main  # Move HEAD to main branch
git checkout feature  # Move HEAD to feature branch
```

---

## 13. AIML Project Git Structure

```
ml-project/
│
├── .git/                    ← Git stores data here
│   ├── objects/
│   ├── refs/
│   └── HEAD
│
├── .gitignore               ← Tell Git what to ignore
│   └── (*.pkl, *.csv, etc)
│
├── src/
│   ├── preprocessing.py     ✓ Track code
│   ├── model.py             ✓ Track code
│   └── train.py             ✓ Track code
│
├── data/
│   ├── small_sample.csv     ✓ Track small files
│   └── raw/                 ✗ Ignore large files
│
├── models/
│   ├── baseline.pkl         ✗ Ignore trained models (add to .gitignore)
│   └── v2.h5                ✗ Ignore trained models
│
├── results/
│   ├── metrics.json         ✓ Track results/metrics
│   └── plots.png            ✓ Track visualizations
│
├── notebooks/
│   └── exploration.ipynb    ✓ Track notebooks
│
└── README.md                ✓ Track documentation
```

---

## 14. GitHub Features Map

```
GitHub Website Interface:

Code Tab
├── See all files
├── View code history
└── Download as ZIP

Issues Tab
├── Report bugs
├── Request features
└── Track discussions

Pull Requests Tab
├── Propose changes
├── Code review
├── Merge features
└── Track discussions

Actions Tab
├── Run tests
├── Deploy code
└── Automate tasks

Settings Tab
├── Repository settings
├── Collaborators
└── GitHub Pages

Insights Tab
└── Graphs and statistics
```

---

## 15. The Complete Git Picture

```
Your Workflow (Simplified):

┌─────────────────────────────────────────────┐
│ 1. PLAN                                     │
│    "I'll add data preprocessing"            │
└─────────────────────────────────────────────┘
         ↓
┌─────────────────────────────────────────────┐
│ 2. CREATE BRANCH                            │
│    git checkout -b feature/preprocessing    │
└─────────────────────────────────────────────┘
         ↓
┌─────────────────────────────────────────────┐
│ 3. CODE                                     │
│    Edit preprocessing.py                    │
│    Add new functions                        │
└─────────────────────────────────────────────┘
         ↓
┌─────────────────────────────────────────────┐
│ 4. STAGE & COMMIT                           │
│    git add preprocessing.py                 │
│    git commit -m "Add preprocessing"        │
└─────────────────────────────────────────────┘
         ↓
┌─────────────────────────────────────────────┐
│ 5. PUSH TO GITHUB                           │
│    git push origin feature/preprocessing    │
└─────────────────────────────────────────────┘
         ↓
┌─────────────────────────────────────────────┐
│ 6. CREATE PULL REQUEST (on GitHub)          │
│    Describe your changes                    │
│    Request code review                      │
└─────────────────────────────────────────────┘
         ↓
┌─────────────────────────────────────────────┐
│ 7. REVIEW & DISCUSS                         │
│    Team reviews your code                   │
│    Make improvements if needed              │
└─────────────────────────────────────────────┘
         ↓
┌─────────────────────────────────────────────┐
│ 8. MERGE                                    │
│    Your feature is merged to main           │
│    It's now part of the project!            │
└─────────────────────────────────────────────┘
```

---

## Quick Reference Diagrams

### Git Command Effects

```
Your Current State: 
main: A──B──C──D
           ↑
         (HEAD)

git checkout -b new:      A──B──C──D
                              ↓
                            new (HEAD)

git merge new:            A──B──C──D──E
                              └────┬┘  (merge commit)

git reset --soft HEAD~1:  A──B──C
                              ↑
                            (HEAD, changes staged)
```

---

**Congratulations!** You now have a visual understanding of how Git works! These concepts apply to every Git project you'll ever use. 🎉
