# Chapter 13: Keep It Clean! — .gitignore

[<< Previous: Trunk-Based Development](12_trunk_based_development.md) | [Next: Best Practices & Cheat Sheet >>](14_best_practices_and_cheatsheet.md)

---

Imagine you just moved into a beautiful new apartment. You unpack your stuff, arrange everything nicely... and then you start throwing trash, junk mail, and old pizza boxes into the living room alongside your furniture. Not great, right?

Your Git repository is your apartment. Some files belong there (your source code, config, docs). Others absolutely do NOT (build artifacts, passwords, logs, `node_modules`). This chapter teaches you how to tell Git: **"Don't even LOOK at these files."** 🙈

## The Problem 🗑️

Without a `.gitignore`, your repo can end up tracking things like:

- `node_modules/` — thousands of dependency files (can be 500MB+!)
- `.env` — your secret API keys and passwords 🔑😱
- `dist/` or `build/` — compiled files that can be regenerated
- `.DS_Store` — macOS metadata files nobody wants
- `*.log` — log files that grow forever
- `__pycache__/` — Python bytecode cache
- `*.class` — compiled Java files

These files clutter your repo, slow down Git, expose secrets, and annoy your teammates. Let's fix that.

> **⚠️ Watch it!**
>
> **Committing secrets is one of the biggest security mistakes in software.** If you accidentally push an API key or password to GitHub (especially a public repo), consider it compromised. Bots scan GitHub 24/7 looking for exposed credentials. A `.gitignore` is your first line of defense.

## What Is `.gitignore`? 📋

`.gitignore` is a special file in your repo's root directory that tells Git which files and folders to **completely ignore**. Git won't track them, won't show them in `git status`, and won't let you accidentally commit them.

The name starts with a dot (`.`) — which makes it a "hidden file" on macOS and Linux. Don't worry, Git sees it just fine.

## Creating Your `.gitignore` 📝

```bash
cd ~/git-practice
```

Create the file:

```bash
cat > .gitignore << 'EOF'
# Dependencies
node_modules/

# Build output
dist/
build/

# Environment variables (SECRETS!)
.env
.env.local
.env.*.local

# OS-generated files
.DS_Store
Thumbs.db

# Editor files
.vscode/settings.json
.idea/

# Logs
*.log
npm-debug.log*

# Compiled files
*.class
*.o
*.pyc
__pycache__/
EOF
```

Now stage and commit it:

```bash
git add .gitignore
git commit -m "Add .gitignore to keep repo clean"
```

> **💡 Pro tip:** The `.gitignore` file itself SHOULD be committed! It's part of your project configuration, and everyone on the team should use the same ignore rules.

## How `.gitignore` Patterns Work 🎯

The syntax is simple but powerful:

| Pattern | What It Ignores | Example |
|---------|----------------|---------|
| `filename` | That exact file in any directory | `debug.log` |
| `*.extension` | All files with that extension | `*.log` |
| `directory/` | An entire directory and everything inside | `node_modules/` |
| `!pattern` | **UN-ignore** something (exception) | `!important.log` |
| `#` | Comment (not a pattern) | `# This is a comment` |
| `**/pattern` | Match in any subdirectory | `**/temp/` |
| `pattern/**` | Match everything inside a directory | `logs/**` |

### Some Examples

```gitignore
# Ignore all .log files
*.log

# But DON'T ignore this specific one
!important.log

# Ignore the entire build folder
build/

# Ignore .env files in any subdirectory
**/.env

# Ignore all .tmp files in the temp directory
temp/*.tmp
```

## Should I Track This File? — The Decision Tree 🌲

Not sure whether a file belongs in your repo? Follow this:

```mermaid
graph TD
    Q1{"Is it source code<br/>you wrote?"} -->|"Yes"| A1["✅ TRACK IT"]
    Q1 -->|"No"| Q2{"Is it a config file<br/>for the project?"}
    Q2 -->|"Yes"| Q3{"Does it contain<br/>secrets or passwords?"}
    Q3 -->|"No"| A2["✅ TRACK IT"]
    Q3 -->|"Yes! 🔑"| A3["🚫 IGNORE IT<br/>Add to .gitignore<br/>Use .env.example instead"]
    Q2 -->|"No"| Q4{"Can it be regenerated<br/>or downloaded?"}
    Q4 -->|"Yes"| A4["🚫 IGNORE IT<br/>(node_modules, build, dist)"]
    Q4 -->|"No"| Q5{"Is it OS/editor<br/>generated?"}
    Q5 -->|"Yes"| A5["🚫 IGNORE IT<br/>(.DS_Store, .idea, Thumbs.db)"]
    Q5 -->|"No"| A6["🤔 Probably track it.<br/>Ask your team."]

    style A1 fill:#51cf66,stroke:#2f9e44,color:#fff
    style A2 fill:#51cf66,stroke:#2f9e44,color:#fff
    style A3 fill:#ff6b6b,stroke:#fa5252,color:#fff
    style A4 fill:#ff6b6b,stroke:#fa5252,color:#fff
    style A5 fill:#ff6b6b,stroke:#fa5252,color:#fff
    style A6 fill:#ffd93d,stroke:#f5c518,color:#333
```

### The Golden Rules

- ✅ **Track:** source code, config (without secrets), documentation, tests, `.gitignore` itself
- 🚫 **Ignore:** dependencies (`node_modules`), build output, secrets (`.env`), OS files, logs, caches

## GitHub's `.gitignore` Templates 📚

Don't write your `.gitignore` from scratch! GitHub maintains a collection of templates for every language and framework:

👉 [github.com/github/gitignore](https://github.com/github/gitignore)

When you create a new repo on GitHub, you can select a `.gitignore` template right from the creation screen. Just pick your language/framework and GitHub generates the file for you.

Common templates:
- **Node** — ignores `node_modules/`, `dist/`, `.env`
- **Python** — ignores `__pycache__/`, `*.pyc`, `venv/`
- **Java** — ignores `*.class`, `target/`, `*.jar`
- **macOS** — ignores `.DS_Store`, `.AppleDouble`

> **💡 There are no dumb questions**
>
> **Q: "I already committed a file and THEN added it to `.gitignore`. It's still being tracked! What do I do?"**
>
> A: `.gitignore` only prevents **new** files from being tracked. Files that are already tracked stay tracked. To stop tracking a file that's already committed:
> ```bash
> git rm --cached filename
> git commit -m "Stop tracking filename"
> ```
> The `--cached` flag removes the file from Git's tracking without deleting it from your disk. After this commit, `.gitignore` will take effect for that file.
>
> **Q: "Can I have a personal `.gitignore` that doesn't affect the team?"**
>
> A: Yes! Add patterns to `.git/info/exclude` — it works exactly like `.gitignore` but isn't committed to the repo. Great for personal editor configs or OS-specific files that only affect you.
>
> **Q: "What if I accidentally committed a secret to GitHub?!"**
>
> A: 😱 Act fast:
> 1. **Rotate the secret immediately** — change the password/API key
> 2. Remove the file and add it to `.gitignore`
> 3. Consider the old secret compromised — even if you remove the commit, it might have been cached/cloned by others
>
> Prevention is much easier than cleanup. Use `.gitignore` from day one!

## The `.env.example` Pattern 📋

For projects that need environment variables, a common pattern is:

1. Add `.env` to `.gitignore` (never commit the real secrets)
2. Create a `.env.example` file with placeholder values (DO commit this)

```bash
# .env (IGNORED — contains real secrets)
DATABASE_URL=postgres://user:s3cr3tpassw0rd@localhost:5432/mydb
API_KEY=sk-abc123realkey456

# .env.example (COMMITTED — shows what variables are needed)
DATABASE_URL=postgres://user:password@localhost:5432/mydb
API_KEY=your-api-key-here
```

This way, new team members know which environment variables they need to set up, without exposing the actual values.

---

## 🏋️ Exercise 13: The Bouncer

**Objective:** Create a `.gitignore`, add files that should be ignored, and verify Git doesn't see them.

**Steps:**

1. Navigate to your practice repo:
   ```bash
   cd ~/git-practice
   ```

2. Create some files that should NOT be in a repo:
   ```bash
   echo "SECRET_KEY=super_secret_123" > .env
   mkdir -p node_modules/fake-package
   echo "module.exports = {}" > node_modules/fake-package/index.js
   echo "Error at line 42" > debug.log
   touch .DS_Store
   ```

3. Check status — Git sees everything:
   ```bash
   git status
   ```
   **Expected:** All those files show up as untracked. 😬

4. Create a `.gitignore` (if you haven't already):
   ```bash
   cat > .gitignore << 'EOF'
   # Secrets
   .env

   # Dependencies
   node_modules/

   # Logs
   *.log

   # OS files
   .DS_Store
   EOF
   ```

5. Check status again:
   ```bash
   git status
   ```
   **Expected:** The ignored files are GONE from the status output! Only `.gitignore` itself shows up as untracked. The bouncer did its job! 🚫

6. Verify — try to add an ignored file:
   ```bash
   git add .env
   ```
   **Expected:**
   ```
   The following paths are ignored by one of your .gitignore files:
   .env
   hint: Use -f if you really want to add them.
   ```
   Git refuses! It's protecting you. 🛡️

7. Commit the `.gitignore`:
   ```bash
   git add .gitignore
   git commit -m "Add .gitignore to protect repo from junk and secrets"
   ```

8. Final status check:
   ```bash
   git status
   ```
   **Expected:** Clean working tree. The "bad" files still exist on your disk, but Git pretends they don't exist. Perfect! ✅

**🎯 What You Learned:**

`.gitignore` acts as a bouncer for your repository — it keeps out files that don't belong. You learned to ignore secrets, dependencies, logs, and OS-generated files. Most importantly, you know that `.gitignore` should be one of the FIRST things you create in any new project. Prevention is always better than cleanup.

---

## 📝 Pop Quiz: Chapter 13

**1. You already committed `.env` to your repo. You then add `.env` to `.gitignore`. Is the file now ignored?**

<details>
<summary>Show answer</summary>

**No!** `.gitignore` only prevents **new, untracked** files from being tracked. Files that are already tracked continue to be tracked. To fix this:

```bash
git rm --cached .env     # Stop tracking (doesn't delete the file)
git commit -m "Stop tracking .env"
```

After this, `.gitignore` will properly ignore `.env` going forward.

</details>

**2. What does `*.log` in `.gitignore` do?**

<details>
<summary>Show answer</summary>

It ignores **all files that end with `.log`** in the entire repository — `debug.log`, `error.log`, `app.log`, etc. The `*` is a wildcard that matches any filename.

</details>

**3. Should you commit the `.gitignore` file itself?**

<details>
<summary>Show answer</summary>

**Yes!** `.gitignore` is a project configuration file that should be shared with your entire team. Commit it so everyone benefits from the same ignore rules. This prevents teammates from accidentally committing `node_modules` or secrets.

</details>

---

🏆 **Level 13 Complete!** Your repo is now protected from junk files, secrets, and clutter. You know how to write ignore patterns, use templates, and handle the tricky case of already-tracked files. One more chapter to go — let's wrap it all up with a cheat sheet and best practices! 🎓

---

[<< Previous: Trunk-Based Development](12_trunk_based_development.md) | [Next: Best Practices & Cheat Sheet >>](14_best_practices_and_cheatsheet.md)
