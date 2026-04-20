# Conventional Commits Guide

This project enforces **Conventional Commits** to maintain a clean, readable commit history and enable automated changelog generation.

## Format

```
<type>(<scope>)?: <subject>

<body>

<footer>
```

### Components:

- **type** (required): Category of the change
- **scope** (optional): Part of the codebase affected
- **subject** (required): Brief description of the change
- **body** (optional): Detailed explanation
- **footer** (optional): References issues or breaking changes

##  Commit Types

| Type       | Description                                         | Example                                  |
| ---------- | --------------------------------------------------- | ---------------------------------------- |
| `feat`     | New feature                                         | `feat(auth): add login functionality`    |
| `fix`      | Bug fix                                             | `fix(user): correct password validation` |
| `docs`     | Documentation changes                               | `docs: update README installation steps` |
| `style`    | Code style changes (formatting, missing semicolons) | `style: format code with prettier`       |
| `refactor` | Code refactoring without feature/fix changes        | `refactor(models): simplify User model`  |
| `perf`     | Performance improvements                            | `perf(database): optimize query`         |
| `test`     | Adding or updating tests                            | `test(auth): add login test cases`       |
| `chore`    | Maintenance tasks, dependencies                     | `chore: update dependencies`             |
| `ci`       | CI/CD configuration changes                         | `ci: update GitHub Actions workflow`     |
| `revert`   | Revert a previous commit                            | `revert: undo feature X`                 |

##  Examples

###  VALID

```bash
# Simple feature
git commit -m "feat: add user registration"

# Feature with scope
git commit -m "feat(auth): add two-factor authentication"

# Bug fix
git commit -m "fix(login): resolve incorrect token validation"

# With body and footer
git commit -m "feat(email): add email verification

Send verification email on user signup.
User must verify before account activation.

Closes #123"

# Breaking change
git commit -m "feat(api)!: change user endpoint response format

BREAKING CHANGE: The user endpoint now returns nested user object"
```

###  INVALID

```bash
#  No type
git commit -m "add new feature"

#  Unclear description
git commit -m "feat: stuff"

#  Wrong type format
git commit -m "Feature: add login"

#  No subject after colon
git commit -m "feat(auth):"

#  Imperative not followed
git commit -m "feat: added authentication"
```

## 🔧 Local Setup (Optional)

Install Husky + commitlint to validate commits locally before pushing:

```bash
# Install packages
npm install --save-dev husky @commitlint/cli @commitlint/config-conventional

# Setup Husky
npx husky install

# Create commit-msg hook
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

Now commits will be validated locally before pushing!

## Commit Message Template

To use a template for every commit:

```bash
git config commit.template .gitmessage
```

Create `.gitmessage` file:

```
<type>(<scope>): <subject>

# <body>

# <footer>

# Type: feat, fix, docs, style, refactor, perf, test, chore, ci, revert
# Scope: (optional) area of the codebase
# Subject: (required) clear, concise description
```

## 🔍 Why Conventional Commits?

 **Automatic Changelog Generation** - Commits are parsed for release notes  
 **Clear History** - Easy to understand what changed and why  
 **Semantic Versioning** - Automatically determine version bumps  
 **Better Collaboration** - Team consistency and professionalism  
 **Tooling Integration** - Works with automated tools and bots

## ⚠️ Validation

All commits in Pull Requests and pushes to `main` are validated by GitHub Actions.

-  Valid commits pass
-  Invalid commits block merging

##  Help

Forgot the format? See examples above or check the **GitHub Actions** tab in your repository for detailed error messages.
