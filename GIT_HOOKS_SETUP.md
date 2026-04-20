# Git Hooks Setup - Local Validation Before Push

This project uses **Husky** to run git hooks that validate your code **locally** before commits and pushes reach GitHub.

## 🎯 What Gets Checked Before Push/Commit

### Pre-Commit Hook (Before `git commit`)

- ✅ Detects `dd()` and `dump()` debug statements
- ✅ Checks PHP syntax errors
- ✅ Warns about `console.log()` in JavaScript files
- ❌ **Blocks commit if critical errors found**

### Pre-Push Hook (Before `git push`)

- ✅ Prevents pushing directly to `main`
- ✅ Validates branch naming convention (feature/, fix/, docs/, etc.)
- ❌ **Blocks push if requirements not met**

---

## 📦 Installation

### 1. Install Dependencies

```bash
npm install
# or
yarn install
```

### 2. Install Husky

```bash
npx husky install
```

### 3. Verify Setup

```bash
# Check if hooks are installed
ls -la .husky/

# You should see:
# -rw-r--r--  pre-commit
# -rw-r--r--  pre-push
```

---

## 🔄 Workflow Example

### Scenario: Making Changes to User Authentication

```bash
# 1. Create feature branch
git checkout -b feature/user-auth

# 2. Make changes
# Edit app/Http/Controllers/AuthController.php
# Add new authentication logic

# 3. Stage your changes
git add app/Http/Controllers/AuthController.php

# 4. Try to commit
git commit -m "feat(auth): add two-factor authentication"

# ✅ Pre-commit hook runs:
# - Checks for dd() statements
# - Validates PHP syntax
# - Checks for debug code
# ✅ If all pass → Commit created

# 5. Try to push
git push origin feature/user-auth

# ✅ Pre-push hook runs:
# - Checks you're not on 'main' branch
# - Validates branch name follows convention
# ✅ If all pass → Push to GitHub
```

---

## ❌ What Will Block Your Push

### Error #1: Leftover `dd()` Statement

```bash
git add app/Models/User.php
git commit -m "fix: user validation"

# ❌ OUTPUT:
# 🔍 Running pre-commit checks...
# ❌ ERROR: Found dd() statements in staged files!
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# ❌ COMMIT BLOCKED: 1 critical error(s) found

# FIX: Remove dd() from file and try again
git add app/Models/User.php
git commit -m "fix: user validation"  # ✅ Now it works!
```

### Error #2: Pushing Directly to Main

```bash
git push origin main

# ❌ OUTPUT:
# ❌ ERROR: You cannot push directly to main!
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# Please follow this workflow:
# 1. Create a feature branch
# 2. Make changes
# 3. Push to feature branch
# 4. Create a Pull Request

# FIX: Use feature branch instead
git checkout -b feature/my-changes
git push origin feature/my-changes  # ✅ Now it works!
```

### Error #3: Invalid Branch Name

```bash
git checkout -b my-changes
git push origin my-changes

# ⚠️  WARNING: Branch name should follow convention
# Current branch: my-changes
# Recommended format: <type>/<description>
# Continue anyway? (y/N):

# CHOOSE: Type 'y' to continue or 'N' to rename branch
# BETTER: Rename to follow convention
git branch -m my-changes feature/my-changes
git push origin feature/my-changes  # ✅ Now it works!
```

---

## 🔓 Bypass (Not Recommended!)

If you really need to skip a hook:

```bash
# Skip pre-commit hook
git commit --no-verify -m "feat: my changes"

# Skip pre-push hook
git push --no-verify origin feature/my-branch
```

**⚠️ Use `--no-verify` only in emergencies!** It bypasses all safety checks.

---

## 🔧 Common Issues

### "Permission denied" Error on Mac/Linux

Make the hooks executable:

```bash
chmod +x .husky/pre-commit
chmod +x .husky/pre-push
```

### Hooks Not Running

Verify installation:

```bash
cat .git/hooks/pre-commit
# Should show: #!/bin/sh
# . "$(dirname "$0")/_/husky.sh"
```

If not, reinstall:

```bash
npx husky uninstall
npx husky install
```

### Need to Update Hooks

Edit files in `.husky/`:

- `.husky/pre-commit` - Edit pre-commit checks
- `.husky/pre-push` - Edit pre-push checks

---

## ✅ Valid Workflow

### Correct Branch Naming

```bash
✅ feature/user-authentication
✅ fix/login-bug
✅ docs/api-documentation
✅ style/code-formatting
✅ refactor/auth-module
✅ perf/database-optimization
✅ test/add-unit-tests
✅ chore/update-dependencies
✅ ci/github-actions-setup
✅ hotfix/critical-security-issue
```

### Correct Commit Flow

1. Create feature branch: `git checkout -b feature/xxx`
2. Make changes and stage: `git add .`
3. Commit with validation: `git commit -m "feat: xxx"` (pre-commit hook runs)
4. Push to feature branch: `git push origin feature/xxx` (pre-push hook runs)
5. Create PR on GitHub
6. Get approved and merge

---

## 📊 Hook Status Check

View your current git hooks:

```bash
ls -la .husky/
```

You should see:

```
pre-commit    (Validates code before commit)
pre-push      (Validates branch before push)
```

---

## 🆘 Help & Support

- **Hooks not working?** Run: `npx husky install`
- **Need to disable hooks?** Run: `npx husky uninstall`
- **Permission issues?** Run: `chmod +x .husky/pre-commit .husky/pre-push`
- **Questions?** Check GitHub Actions logs or ask maintainers

---

**Remember**: These hooks are designed to catch issues early and prevent bad code from reaching GitHub! 🛡️
