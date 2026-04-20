# Branch Protection Policy

This repository enforces a strict branch protection policy to maintain code quality and ensure all changes go through proper review.

## Rules Enforced

### 1. **No Direct Pushes to `main`**
- All changes to `main` must go through Pull Requests (PRs)
- Direct commits to `main` will be automatically rejected by the GitHub Action

### 2. **Feature Branch Required**
- You must create a feature branch first (e.g., `feature/user-auth`, `fix/login-bug`, `docs/readme`)
- PRs cannot originate from `main` branch
- Branch naming convention: `<type>/<description>` (feature, fix, docs, etc.)

## Workflow

### For Contributors:

1. **Create a Feature Branch**
   ```bash
   git checkout -b feature/my-feature
   # or
   git checkout -b fix/bug-name
   ```

2. **Make Your Changes**
   ```bash
   git add .
   git commit -m "Your descriptive commit message"
   ```

3. **Push to Feature Branch**
   ```bash
   git push origin feature/my-feature
   ```

4. **Create a Pull Request**
   - Go to GitHub and create a PR from your feature branch to `main`
   - Add description of changes
   - Request reviewers if needed

5. **Wait for Checks**
   - GitHub Action will validate your PR
   - Your branch must pass all checks before merging

6. **Merge to Main**
   - Once approved, PR can be merged to `main`
   - Delete your feature branch after merging

##  Will NOT Work

```bash
#  Direct push to main (will be rejected)
git push origin main

# PR from main to main (will fail check)
# This is blocked by the branch-protection.yml workflow
```

## What WILL Work

```bash
# Create and push to feature branch
git checkout -b feature/my-changes
git push origin feature/my-changes

# Create PR from feature branch to main
# (allowed through GitHub UI)
```

## GitHub Action Details

The workflow file `.github/workflows/branch-protection.yml` performs:

-  all PRs originate from feature branches (not main)
- Prevents any direct pushes to main
- Provides clear error messages when rules are violated

## Recommended Additional GitHub Settings

For complete protection, enable these in repository settings:

1. **Settings → Branches → Add Rule → main**
   -  Require a pull request before merging
   -  Require status checks to pass before merging
   -  Require branches to be up to date before merging
   -  Dismiss stale pull request approvals when new commits are pushed
   -  Require approval of the latest reviewers

##  Issues or Questions?

If you're blocked or have questions about the branch policy, contact the project maintainers.
