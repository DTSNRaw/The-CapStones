# WEEK 1: WORKFLOW BOOTCAMP
## Learning The Sacred Flow Through Doing

**Time to complete:** 2-3 hours total  
**Clearance:** INFRARED (Acceleration Track)  
**Deliverables:** 3 GitHub Issues created, 1 Pull Request merged

---

## THE BIG PICTURE

This week you will create three GitHub Issues and complete one of them via Pull Request. This teaches you the entire workflow that every contribution will follow for the rest of the semester:

```
┌─────────────────────────────────────────────────────────────────────┐
│                        THE SACRED FLOW                              │
│                                                                     │
│    ISSUE ──► BRANCH ──► CODE ──► PULL REQUEST ──► REVIEW ──► MERGE │
│      │                                              ▲               │
│      │                                              │               │
│      └───────────── nothing skips this ─────────────┘               │
└─────────────────────────────────────────────────────────────────────┘
```

Every feature. Every bug fix. Every documentation change. No exceptions.

---

## PART 1: CREATE YOUR FUTURE ISSUES (30 min)

Before any code, you'll create two issues for work that's coming later. This teaches you that **issues come first** and that some issues stay open for weeks.

### Issue A: "Create Pitch Presentation"

**Navigate to:** Your team repository → Issues → New Issue

**Title:** `Create Pitch Presentation`

**Body:**
```markdown
## What
Create team pitch presentation for Week 4 stakeholder review.

## Why
Required milestone deliverable. First formal evaluation.

## Acceptance Criteria
- [ ] Slides created covering required sections
- [ ] All team members have speaking parts
- [ ] Demo working and rehearsed
- [ ] Timing under 10 minutes
- [ ] Architecture diagram included

## Notes
See Pitch Presentation Rubric for detailed requirements.
```

**After creating:**
1. Assign the **Milestone:** `Milestone 2 (Feb-Mar)` *(or your equivalent)*
2. Add **Label:** `presentation` or `documentation`
3. **Do not assign** to yourself (this is a team deliverable)

---

### Issue B: "Apply for Graduation"

This is an **out-of-character** but **critical** real-world task.

**Title:** `Apply for Graduation`

**Body:**
```markdown
## What
Submit graduation application through student portal.

## Why
Missing the deadline = not graduating on time.

## Acceptance Criteria
- [ ] Application submitted via WebAdvisor/student portal
- [ ] Confirmation screenshot saved
- [ ] Advisor notified if required

## Deadline
Check your student portal for exact date. Usually 6-8 weeks before end of semester.

## Out of Character Note
This is not part of the simulation. This is your actual graduation. Don't forget.
```

**After creating:**
1. Assign the **Milestone:** `Milestone 2 (Feb-Mar)`
2. Assign **to yourself**
3. Add **Label:** `personal` or `OOC`

---

**Checkpoint:** You should now have two open issues in your repository.

```
YOUR REPO
├── Issues
│   ├── #1 Create Pitch Presentation    [Open] → Milestone 2
│   └── #2 Apply for Graduation         [Open] → Milestone 2
```

These will stay open until you complete them. That's normal.

---

## PART 2: THE COMPLETE CYCLE (60-90 min)

Now you'll do the full Sacred Flow: create an issue, branch, code, and submit a PR.

### Step 1: Create the Issue

**Title:** `Validate Dev Environment - [Your Name]`

**Body:**
```markdown
## What
Verify development environment is operational by running test script.

## Why
Blocked on all future work until environment confirmed working.

## Acceptance Criteria
- [ ] CodeSpace launches successfully
- [ ] Python runs without errors
- [ ] Test script produces expected output
- [ ] Verification proof captured

## Technical Notes
- Using GitHub Codespaces
- Python 3.x required
- Expected output: "Environment validated for [name]"
```

**After creating:**
1. Assign **Milestone:** `Milestone 1 (Jan-Feb)`
2. Assign **to yourself**
3. Add **Label:** `setup` or `infrastructure`
4. **Note the issue number** (probably #3)

---

### Step 2: Create Your Branch

Open your repository in Codespaces (or clone locally).

```bash
# Make sure you're on main and up to date
git checkout main
git pull origin main

# Create your branch (replace 3 with your issue number)
git checkout -b feature/3-validate-environment-yourname
```

**Branch naming pattern:**
```
feature/[issue#]-[short-description]
   │        │            │
   │        │            └── what you're doing
   │        └── links to the issue
   └── type of work (feature, fix, docs)
```

---

### Step 3: Create the Test Script

Create a new file: `verify_environment.py`

```python
#!/usr/bin/env python3
"""
Environment Validation Script
Run this to confirm your development environment is operational.
"""

import sys
import platform
from datetime import datetime

def validate_environment():
    """Run basic environment checks and report status."""
    
    print("=" * 50)
    print("ALGOCRATIC FUTURES™ ENVIRONMENT VALIDATION")
    print("=" * 50)
    print()
    
    # Collect system info
    checks = {
        "Python Version": sys.version.split()[0],
        "Platform": platform.system(),
        "Machine": platform.machine(),
        "Timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    }
    
    # Display results
    print("SYSTEM DIAGNOSTICS:")
    for key, value in checks.items():
        print(f"  {key}: {value}")
    print()
    
    # Version check
    major, minor = sys.version_info[:2]
    if major >= 3 and minor >= 8:
        status = "PASSED"
        message = "Environment validated successfully."
    else:
        status = "WARNING"
        message = f"Python {major}.{minor} detected. 3.8+ recommended."
    
    print(f"STATUS: {status}")
    print(f"MESSAGE: {message}")
    print()
    
    # The proof of completion
    # TODO: Replace YOUR_NAME with your actual name
    print("=" * 50)
    print("Environment validated for: YOUR_NAME")
    print("=" * 50)
    
    return status == "PASSED"

if __name__ == "__main__":
    success = validate_environment()
    sys.exit(0 if success else 1)
```

**Your task:** 
1. Replace `YOUR_NAME` with your actual name
2. Run the script: `python verify_environment.py`
3. Screenshot or copy the output

---

### Step 4: Commit Your Work

```bash
# Stage your new file
git add verify_environment.py

# Commit with a meaningful message
git commit -m "Add environment validation script

Refs #3

- Checks Python version and platform
- Reports system diagnostics
- Confirms environment operational"
```

**Commit message anatomy:**
```
feat(setup): add environment validation script   ← subject line
                                                 ← blank line
Refs #3                                          ← issue reference

- Checks Python version and platform             ← bullet points
- Reports system diagnostics                        explaining what
- Confirms environment operational                  you did
```

---

### Step 5: Push Your Branch

```bash
git push origin feature/3-validate-environment-yourname
```

If this is your first push, Git may ask you to set upstream:
```bash
git push --set-upstream origin feature/3-validate-environment-yourname
```

---

### Step 6: Create Pull Request

**Navigate to:** Your repository on GitHub → You'll see a prompt to create a PR

**Title:** `Add environment validation script`

**Body:**
```markdown
## Summary
Created environment validation script to confirm dev setup is operational.

## Closes
Closes #3

## Changes
- Added `verify_environment.py` script
- Script checks Python version, platform, and outputs confirmation

## Testing Done
- [x] Script runs without errors
- [x] Output shows "Environment validated for [my name]"
- [x] Python 3.8+ confirmed

## Screenshot/Output
[Paste your output here]
```

**Important:** The text `Closes #3` is magic syntax. When this PR merges, it automatically closes Issue #3.

---

### Step 7: Request Review

1. On the PR page, find "Reviewers" in the right sidebar
2. Request review from a teammate (or instructor if solo)
3. Wait for approval

**While waiting:** Review a teammate's PR if one is available. This is how you learn the codebase faster.

---

### Step 8: Merge and Clean Up

Once approved:

1. Click **"Squash and merge"** (keeps history clean)
2. Confirm the merge
3. **Delete the branch** (GitHub offers this button after merge)
4. Verify Issue #3 is now closed

---

## WORKFLOW COMPLETE 🎉

You've just completed one full cycle:

```
┌────────────────────────────────────────────────────────────────────┐
│                                                                    │
│  ✓ Issue #3 Created         → Defined the work                    │
│  ✓ Branch Created           → Isolated your changes               │
│  ✓ Code Written             → Did the actual work                 │
│  ✓ Commit Made              → Saved with meaningful message       │
│  ✓ Push to Remote           → Shared with team                    │
│  ✓ Pull Request Opened      → Requested review                    │
│  ✓ Review Received          → Got feedback (or approval)          │
│  ✓ Merged to Main           → Work is now official                │
│  ✓ Branch Deleted           → Cleaned up                          │
│  ✓ Issue Auto-Closed        → "Closes #3" magic                   │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

**Every contribution for the rest of the semester follows this pattern.**

---

## WEEK 1 COMPLETION CHECKLIST

Submit evidence of completion:

- [ ] **Issue A created:** "Create Pitch Presentation" (link: _______)
- [ ] **Issue B created:** "Apply for Graduation" (link: _______)
- [ ] **Issue C created + closed via PR:** "Validate Dev Environment" (PR link: _______)
- [ ] **Screenshot:** Output of `verify_environment.py` showing your name

---

## WHAT'S DIFFERENT FOR REAL WORK

This exercise was self-contained. Real work has extra steps:

| This Exercise | Real Feature Work |
|---------------|-------------------|
| Solo work | Coordinate with team |
| New file | Changes to existing code |
| One commit | Multiple commits as you iterate |
| Quick review | May need revisions |
| Auto-approve (maybe) | Substantive review feedback |

The pattern stays the same. The stakes increase.

---

## QUICK REFERENCE CARD

```
┌─────────────────────────────────────────────────────────────┐
│ GIT COMMAND CHEAT SHEET                                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│ START NEW WORK                                              │
│   git checkout main                                         │
│   git pull origin main                                      │
│   git checkout -b feature/[issue#]-[description]            │
│                                                             │
│ SAVE PROGRESS                                               │
│   git add [files]      or    git add .                      │
│   git commit -m "message"                                   │
│   git push origin [branch-name]                             │
│                                                             │
│ CHECK STATUS                                                │
│   git status           (what's changed?)                    │
│   git log --oneline    (commit history)                     │
│   git branch           (which branch am I on?)              │
│                                                             │
│ AFTER PR MERGES                                             │
│   git checkout main                                         │
│   git pull origin main                                      │
│   (delete your local branch if you want)                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## STUCK?

| Problem | Solution |
|---------|----------|
| Can't create branch | Make sure you're on main first: `git checkout main` |
| Push rejected | Pull first: `git pull origin main`, resolve conflicts |
| Wrong branch name | Rename: `git branch -m old-name new-name` |
| Committed to wrong branch | Don't panic. Ask for help. It's fixable. |
| CodeSpace won't load | Try refreshing. Check GitHub status page. Ask instructor. |

**Remember:** Asking for help is a skill. Practice it now while the stakes are low.

---

*The Algorithm measures commits. Your growth is measured in iterations.*

**frotz** → **plugh**
