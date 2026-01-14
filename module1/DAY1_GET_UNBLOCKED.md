# DAY 1: GET UNBLOCKED
## The 30-Minute Survival Protocol

**If you only have 30 minutes, do exactly this. Nothing else.**

---

## MINUTE 0-5: GET INTO YOUR REPO

```
Your Team Repository → Code → Codespaces → Create codespace on main
```

Wait for it to load. Get coffee.

---

## MINUTE 5-10: PROVE PYTHON WORKS

In the terminal:

```bash
python3 --version
```

See a version number? You're fine. Move on.

---

## MINUTE 10-20: CREATE YOUR FIRST ISSUE

**GitHub → Issues → New Issue**

```
Title: Validate Dev Environment - [Your Name]

Body:
Confirm environment is operational.
- [ ] Codespace launches
- [ ] Python runs
- [ ] First PR submitted
```

Click **Submit**. Note the issue number (probably #1, #2, or #3).

---

## MINUTE 20-30: FIRST CONTRIBUTION

```bash
# In your Codespace terminal:

git checkout main
git pull origin main
git checkout -b feature/[issue#]-setup-yourname

# Create a file
echo "# [Your Name] - Environment Confirmed" > verification-yourname.md
echo "Python: $(python3 --version)" >> verification-yourname.md
echo "Date: $(date)" >> verification-yourname.md

# Save it
git add verification-yourname.md
git commit -m "Add environment verification - Refs #[issue#]"
git push origin feature/[issue#]-setup-yourname
```

Go to GitHub. Create Pull Request. Request review from teammate.

---

## DONE

You have:
- [x] Working environment
- [x] First issue created  
- [x] First branch pushed
- [x] First PR open

**You are no longer blocked.**

Now go read the full bootcamp doc when you have time.

---

```
┌─────────────────────────────────────────┐
│                                         │
│  30 MIN COMPLETE = TEAM INTEGRATION     │
│                                         │
│  Issue ✓  Branch ✓  PR ✓  = You exist   │
│                                         │
└─────────────────────────────────────────┘
```
