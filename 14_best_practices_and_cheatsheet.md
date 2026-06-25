# Chapter 14: You're a Git Pro Now! — Best Practices & Cheat Sheet

[<< Previous: .gitignore](13_gitignore.md) | [Next: Troubleshooting Appendix >>](15_appendix_troubleshooting.md)

---

You made it. You actually made it! 🎉

You've gone from "What is Git?" to branching, merging, resolving conflicts, pushing to GitHub, and understanding trunk-based development. That's an incredible journey. Give yourself a pat on the back. Or a high five. Or a taco. You've earned it. 🌮

This final chapter is your **take-away toolkit** — the best practices to keep you out of trouble, and a cheat sheet you can bookmark and come back to whenever you forget a command (you will, and that's totally fine).

## The 10 Git Commandments 📜

These are the rules that separate "I know Git" from "I'm good at Git." Tape them to your monitor if you need to.

### 1. Commit Often, Commit Small 📦

Each commit should be one logical change. Not "everything I did today." Not "a week's worth of work." One thing.

| ❌ Bad | ✅ Good |
|--------|---------|
| `"Monday's work"` | `"Add login form validation"` |
| `"Updates"` | `"Fix crash when email field is empty"` |
| `"Lots of changes"` | `"Refactor user service to use async/await"` |

Small commits make your history readable, your code reviewable, and your bugs findable.

### 2. Write Meaningful Commit Messages ✍️

Your commit message should complete the sentence: *"If applied, this commit will..."*

```
✅ "Add password strength indicator to signup form"
✅ "Fix race condition in payment processing"
✅ "Remove deprecated API endpoints"

❌ "fix"
❌ "stuff"
❌ "WIP"
❌ "asdfasdf"
```

Future-you will read these messages. Be kind to future-you.

### 3. Pull Before You Push ⬇️⬆️

Always `git pull` before `git push`. This ensures you have the latest changes from your team and prevents rejected pushes.

```bash
git pull
# resolve any conflicts if needed
git push
```

Make it a habit. It takes 2 seconds and saves 20 minutes of confusion.

### 4. Never Commit Secrets 🔐

API keys, passwords, database credentials, private keys — **none of these belong in Git.** Ever. Not even for "just a second."

- Use `.gitignore` to exclude `.env` files
- Use environment variables for secrets
- Use `.env.example` to document what variables are needed

If you accidentally commit a secret, **rotate it immediately**. Consider it compromised.

### 5. Use `.gitignore` From Day One 📋

The very first commit in any new project should include a `.gitignore`. Don't wait until you've already committed `node_modules/` or `.DS_Store`.

Use a template from [github.com/github/gitignore](https://github.com/github/gitignore).

### 6. Use Branches for Everything 🌿

Never commit directly to `main` (except for the initial setup). Always:
1. Create a branch
2. Do your work
3. Open a PR
4. Get it reviewed
5. Merge

This protects `main` and gives you a safety net.

### 7. Keep Branches Short-Lived 🦋

Branch → small change → merge → delete. Repeat. Don't let branches grow old.

- ✅ Hours to 1-2 days
- ❌ Weeks or months

### 8. Review Before You Commit 👀

Always check what you're about to commit:

```bash
git status          # What files changed?
git diff            # What exactly changed?
git diff --staged   # What am I about to commit?
```

This 10-second habit catches accidental commits of debug code, todo comments, and files that shouldn't be tracked.

### 9. Don't Rewrite Shared History 🚫

If a commit has been pushed and other people might have pulled it — **don't rewrite it**. Use `git revert`, not `git reset --hard` or `git push --force`.

Rewriting shared history breaks everyone's setup. Be a good teammate.

### 10. When In Doubt, `git status` 💛

Lost? Confused? Not sure what's going on? Run `git status`. It always tells you:
- What branch you're on
- What files have changed
- What's staged
- What to do next (read the hints!)

`git status` is your compass. Use it liberally.

---

## Command Cheat Sheet 📑

### Setup & Config

| Command | What It Does |
|---------|-------------|
| `git --version` | Check Git version |
| `git config --global user.name "Name"` | Set your name |
| `git config --global user.email "email"` | Set your email |
| `git config --global init.defaultBranch main` | Set default branch name |
| `git config --list` | Show all config settings |

### Creating & Cloning Repos

| Command | What It Does |
|---------|-------------|
| `git init` | Initialize a new repo in current folder |
| `git clone <url>` | Download a repo from a remote |

### Basic Workflow

| Command | What It Does |
|---------|-------------|
| `git status` | Show current state of the repo |
| `git add <file>` | Stage a file for the next commit |
| `git add .` | Stage ALL changes |
| `git commit -m "message"` | Commit staged changes with a message |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |

### History

| Command | What It Does |
|---------|-------------|
| `git log` | Show full commit history |
| `git log --oneline` | Compact commit history |
| `git log --oneline --graph --all` | Visual branch graph |
| `git log -n 5` | Show last 5 commits |
| `git log -- <file>` | Show commits that changed a file |
| `git show <hash>` | Show details of one commit |

### Branching

| Command | What It Does |
|---------|-------------|
| `git branch` | List all local branches |
| `git branch <name>` | Create a new branch |
| `git switch <name>` | Switch to a branch |
| `git switch -c <name>` | Create AND switch to a new branch |
| `git branch -d <name>` | Delete a merged branch |
| `git branch -D <name>` | Force-delete a branch |

### Merging

| Command | What It Does |
|---------|-------------|
| `git merge <branch>` | Merge a branch into current branch |
| `git merge --abort` | Cancel a merge in progress |

### Undoing Things

| Command | What It Does |
|---------|-------------|
| `git restore <file>` | Discard working directory changes |
| `git restore --staged <file>` | Unstage a file (keep changes) |
| `git revert <hash>` | Create a new commit that undoes an old one |

### Remotes & Syncing

| Command | What It Does |
|---------|-------------|
| `git remote -v` | Show remote URLs |
| `git remote add origin <url>` | Add a remote |
| `git push -u origin main` | Push branch (first time, with tracking) |
| `git push` | Push commits to remote |
| `git pull` | Download and merge remote changes |
| `git fetch` | Download remote changes without merging |

---

## Glossary 📖

All the Git terms you've learned, in one place:

| Term | Definition |
|------|-----------|
| **Repository (repo)** | A folder tracked by Git. Contains your project files and the `.git` folder with all history. |
| **Commit** | A snapshot of your project at a specific moment. Has a unique hash, author, date, and message. |
| **Staging area** | The "prep zone" between your working directory and the repository. Files here are ready to be committed. |
| **Working directory** | Your project folder as it exists on disk right now. Where you edit files. |
| **Branch** | A lightweight pointer to a commit. Used to work on features in parallel. |
| **main** | The default branch — conventionally the "source of truth" for your project. |
| **HEAD** | A pointer to your current commit/branch. "You are here." |
| **Merge** | Combining changes from one branch into another. |
| **Merge conflict** | When two branches changed the same lines in the same file. Requires manual resolution. |
| **Remote** | A copy of your repo on another computer (usually GitHub). |
| **Origin** | The default name for your primary remote. |
| **Clone** | Downloading a complete copy of a remote repository. |
| **Push** | Uploading your local commits to a remote. |
| **Pull** | Downloading and merging remote commits into your local branch. |
| **Fetch** | Downloading remote data without merging. |
| **Pull Request (PR)** | A request to merge your branch into another (usually `main`). Includes code review. |
| **Fast-forward merge** | A merge where Git just moves the pointer forward (no divergence). |
| **Three-way merge** | A merge where both branches diverged — Git creates a merge commit. |
| **Hash** | A unique 40-character identifier for each commit (SHA-1). |
| **.gitignore** | A file that tells Git which files to ignore. |
| **Trunk-based development** | A workflow where `main` is always deployable and branches are short-lived. |
| **Feature flag** | A toggle that hides unfinished features in production code. |

---

## Where to Go Next 🗺️

You've built a solid Git foundation. Here's where to level up:

### Concepts to Explore Next
- **Stashing** (`git stash`) — temporarily save changes without committing
- **Rebasing** (`git rebase`) — an alternative to merging that creates a cleaner history
- **Cherry-picking** (`git cherry-pick`) — grab a specific commit from another branch
- **Tagging** (`git tag`) — mark specific commits (like releases: `v1.0.0`)
- **Interactive rebase** — rewrite, squash, and reorder commits
- **Git hooks** — scripts that run automatically before/after Git events
- **Submodules** — repos inside repos

### Resources
- [Pro Git Book](https://git-scm.com/book) — free, comprehensive, the official reference
- [GitHub Skills](https://skills.github.com/) — interactive courses directly on GitHub
- [Oh My Git!](https://ohmygit.org/) — a game that teaches Git visually
- [Learn Git Branching](https://learngitbranching.js.org/) — interactive branching visualizer
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials) — well-written guides with diagrams

### Practice
The best way to learn Git is to use it every day. Start a personal project, contribute to open source, or just use it for your notes and documents. The commands will become muscle memory before you know it.

---

## 🎉 Congratulations!

You did it! You went from *"What is Git?"* to understanding:

- ✅ Why version control exists
- ✅ What Git is and how it thinks (distributed, snapshots)
- ✅ The three areas (working directory, staging, repository)
- ✅ The core workflow (add → commit → push)
- ✅ How to explore history (log, show, diff)
- ✅ How to undo mistakes (restore, revert)
- ✅ Branching and merging
- ✅ Merge conflict resolution
- ✅ Working with remotes (GitHub)
- ✅ Trunk-based development
- ✅ Keeping your repo clean (.gitignore)
- ✅ Best practices that real teams follow

**That's a LOT.** You should genuinely be proud. Most developers don't learn Git this thoroughly — they just wing it and Google commands when things go wrong. You've built real understanding.

The journey doesn't end here. Git has depths you can explore for years. But you now have everything you need to use Git confidently, collaborate with a team, and never lose your work again.

Now go build something amazing. And commit it. 🚀

---

🏆 **FINAL LEVEL COMPLETE!** 🏆

---

[<< Previous: .gitignore](13_gitignore.md) | [Next: Troubleshooting Appendix >>](15_appendix_troubleshooting.md)
