# Code Validation & Error Detection Policy

This project enforces strict error detection and validation for **all users**, including the repository **owner**. No one can bypass code quality checks.

## 🔍 Automated Checks

All commits and pull requests are automatically validated for code quality and errors.

### ✅ Checks Performed

1. **PHP Syntax Validation**
    - Checks all PHP files in `app/` and `routes/` directories
    - Prevents syntax errors from being committed

2. **Laravel Configuration**
    - Validates `config:cache` generation
    - Catches configuration errors early

3. **Database Migrations**
    - Checks migration status and integrity
    - Prevents invalid migrations

4. **Route Validation**
    - Validates all defined routes
    - Detects routing errors

5. **Unit & Feature Tests**
    - Runs PHPUnit if `phpunit.xml` exists
    - Blocks commits if tests fail

6. **Common Code Issues**
    - Detects leftover `dd()` statements ❌
    - Warns about `console.log()` statements ⚠️
    - Detects commented-out code ⚠️

## 👥 Role-Based Enforcement

| Role                 | Branch Requirements        | Error Checks           | Can Bypass |
| -------------------- | -------------------------- | ---------------------- | ---------- |
| **Collaborator**     | ✅ Feature branch required | ✅ All checks enforced | ❌ No      |
| **Repository Owner** | ✅ Feature branch required | ✅ All checks enforced | ❌ No      |

### All Users: ✅ Required

```bash
git checkout -b feature/my-feature
# Make changes
git commit -m "feat: my changes"
git push origin feature/my-feature
# Create PR → Pass all checks → Merge
```

## 🚫 What Gets Blocked for Everyone

### ❌ PHP Syntax Errors

```php
// ❌ This will block the commit
function test( {
    dd($data);
}
```

### ❌ Leftover Debug Code

```php
// ❌ Will be caught and blocked
dd($user);
dump($data);
```

### ❌ Configuration Errors

```php
// ❌ Invalid config structure
'database' => [
    'default' => env('DB_CONNECTION'
    // Missing closing paren
]
```

### ❌ Test Failures

```bash
# ❌ If tests fail, commit is blocked
vendor/bin/phpunit  # If this fails → commit blocked
```

### ❌ Direct Push to Main

```bash
# ❌ No one can push directly to main (owner or collaborators)
git push origin main  # Blocked!
# ✅ Must create feature branch + PR instead
```

## ✅ What Passes Validation

### ✅ Valid Feature Branch Commit

```bash
git checkout -b feat/user-authentication
php artisan make:controller AuthController
# ... make changes ...
git commit -m "feat(auth): add login controller"
git push origin feat/user-authentication
# Create PR → All checks pass ✅
```

### ✅ Working Tests

```bash
# All tests pass
vendor/bin/phpunit
# Commits allowed ✅
```

### ✅ Clean Code

```php
// ✅ No syntax errors, no dd(), no debug code
public function authenticate(Request $request)
{
    $validated = $request->validate([
        'email' => 'required|email',
        'password' => 'required',
    ]);

    return Auth::attempt($validated);
}
```

## 🔧 Local Pre-Commit Validation (Optional)

Run checks locally before pushing:

```bash
# Run PHP linting
find app routes -name "*.php" -type f -exec php -l {} \;

# Run tests
vendor/bin/phpunit

# Search for dd() statements
grep -r "dd(" app routes
```

## 📋 Workflow for All Contributors

1. **Create Feature Branch**

    ```bash
    git checkout -b feat/new-feature
    ```

2. **Make Changes**
    - Write/modify code
    - Run tests locally: `php artisan test`
    - Check for syntax: `php -l file.php`
    - Remove debug code: `dd()`, `dump()`, `console.log()`

3. **Commit with Conventional Commits**

    ```bash
    git commit -m "feat(module): description"
    ```

4. **Push to Feature Branch**

    ```bash
    git push origin feat/new-feature
    ```

5. **Wait for GitHub Actions**
    - Automated validation runs
    - All checks must pass ✅
    - If fails ❌, fix issues and push again

6. **Create Pull Request**
    - Describe changes
    - Link related issues
    - Request reviewer

7. **Merge After Approval**
    - PR is merged to `main`
    - Feature branch can be deleted

## ⚠️ Common Issues

### "Validation failed" Error

**Cause**: One or more checks failed
**Solution**:

1. Read the GitHub Actions log
2. Fix the reported issue locally
3. Commit and push again

### PHP Syntax Error

```
Parse error: syntax error, unexpected '{'
```

**Fix**: Check your PHP file syntax

### Test Failure

```
FAILED: Test failed
```

**Fix**: Run tests locally, fix issues, commit again

### Leftover dd() Statement

```
❌ Found dd() statements in code!
```

**Fix**: Remove `dd()` from your code

## 📞 Support

If you're blocked and unsure why:

1. Check GitHub Actions logs (repository → Actions tab)
2. Run checks locally first
3. Contact repository maintainers

---

**Everyone** (Owner & Collaborators): Must follow all rules and pass all checks ✅
