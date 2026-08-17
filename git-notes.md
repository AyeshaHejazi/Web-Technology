# Git Notes

## 1. Core Workflow

**Concept:** Git tracks changes to files over time through a staging → commit → push cycle.

**Syntax:**
```bash
git status                 # see what's changed
git add filename.html      # stage a specific file
git add .                  # stage everything changed
git commit -m "message"    # save a snapshot with a message
git push                   # send commits to GitHub
git pull                   # fetch + merge latest changes from GitHub
```

**Gotchas:**
- `git add .` stages *everything*, including files you might not want (e.g. `.env`, `node_modules`) — check `git status` first.
- Forgetting to `git pull` before starting new work on a shared repo can cause avoidable merge conflicts.

---

## 2. Commit Messages

**Concept:** Clear commit messages make your history readable — useful for you later, and for anyone reviewing your repo (including recruiters).

**Convention (widely used):**
```
<type>: <short summary>

feat: add student feedback form
fix: correct email validation regex
docs: update README with notes structure
style: format CSS with consistent indentation
refactor: simplify binary search function
```

**Gotchas:**
- "Update stuff" / "fix" / "asdf" as commit messages tell nobody anything six months later — including you.
- Keep the summary line under ~50 characters; add detail in a second paragraph if needed.

---

## 3. Branches

**Concept:** Branches let you work on a feature/experiment without touching the main codebase.

**Syntax:**
```bash
git branch feature-name        # create a branch
git checkout feature-name      # switch to it
git checkout -b feature-name   # create + switch in one step

git checkout main              # go back to main
git merge feature-name         # merge feature-name into current branch
git branch -d feature-name     # delete branch after merging
```

**Gotchas:**
- Always check which branch you're on (`git branch` shows the current one with `*`) before committing — easy to accidentally commit to `main` directly.
- Deleting a branch with unmerged work (`-d`) will warn you; forcing it (`-D`) deletes without warning.

---

## 4. .gitignore

**Concept:** Tells Git which files/folders to never track (secrets, build output, dependencies).

**Syntax:**
```
node_modules/
.env
*.log
.DS_Store
dist/
```

**Gotchas:**
- Adding a file to `.gitignore` *after* it's already been committed won't untrack it — you need `git rm --cached filename` first, then commit.
- Never commit `.env` files or API keys — even in a private repo, treat secrets as if they'll leak.

---

## 5. Undoing Things

**Concept:** Git offers different ways to undo depending on whether changes are staged, committed, or pushed.

**Syntax:**
```bash
git checkout -- filename.html   # discard uncommitted changes to a file
git reset HEAD filename.html    # unstage a file (keep the changes)
git revert <commit-hash>        # safely undo a commit by creating a new "undo" commit
git reset --soft HEAD~1         # undo last commit, keep changes staged
```

**Gotchas:**
- `git reset --hard` permanently discards changes — no undo after that. Use with caution.
- Once a commit is pushed and others may have pulled it, prefer `revert` over `reset` — rewriting shared history causes conflicts for collaborators.

---

## 6. Resolving Merge Conflicts

**Concept:** Happens when Git can't automatically combine changes to the same lines in a file.

**What it looks like:**
```
<<<<<<< HEAD
your version of the line
=======
incoming version of the line
>>>>>>> branch-name
```

**Steps:**
1. Open the conflicted file, decide which version (or combination) to keep.
2. Delete the `<<<<<<<`, `=======`, `>>>>>>>` markers.
3. `git add` the resolved file.
4. `git commit` to finalize the merge.

**Gotchas:**
- Don't panic-delete one whole side without reading both — you can lose real work.
- Conflicts are far less scary with frequent small commits and pulls, rather than huge infrequent ones.

---

## Quick Reference: Common Mistakes
- Committing directly to `main` for experimental changes instead of using a branch.
- Vague commit messages that don't explain *why* a change was made.
- Accidentally committing secrets or `node_modules` because `.gitignore` wasn't set up first.
- Using `git push --force` without understanding it can overwrite others' work.
