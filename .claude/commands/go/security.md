# Security Audit & Hardening

Perform a thorough security review of the codebase, identify vulnerabilities, and apply fixes.

## Usage
`/go/security [target]` - Run security audit on specified target or entire project

## Steps

1. **Scope Assessment**
   - Identify what `$ARGUMENTS` specifies (file, directory, feature, or full audit)
   - If no argument, default to full project security scan
   - Check for existing security configs (`.snyk`, `audit-ci.json`, etc.)

2. **Dependency Vulnerability Scan**
   - Run `npm audit --json` and capture output
   - Parse results for critical and high severity issues
   - Check for outdated packages with known CVEs
   - Review `package-lock.json` for transitive dependency risks

3. **Static Code Analysis**
   - Scan for hardcoded secrets, API keys, tokens, or passwords
   - Look for patterns like `password =`, `secret =`, `api_key =`, `token =`
   - Check `.env` files are in `.gitignore`
   - Identify any credentials accidentally committed

4. **Input Validation Review**
   - Check all user inputs are validated and sanitized
   - Look for SQL injection risks (string concatenation in queries)
   - Identify XSS vulnerabilities (unescaped user content in HTML)
   - Review path traversal risks in file operations
   - Check for prototype pollution vulnerabilities

5. **Authentication & Authorization**
   - Review JWT implementation (algorithm, expiry, validation)
   - Check for missing authorization checks on sensitive routes
   - Verify session management is secure
   - Look for insecure direct object references (IDOR)

6. **Dependency & Supply Chain**
   - Check for packages with unusual permissions or post-install scripts
   - Verify package integrity with lockfile
   - Look for typosquatting risks in dependency names

7. **Secrets & Configuration**
   - Ensure no secrets in source code or config files
   - Verify environment variables are used for sensitive config
   - Check that error messages don't leak sensitive information
   - Review logging to ensure no PII or secrets are logged

8. **Fix & Remediate**
   - For each finding, apply the appropriate fix:
     - Run `npm audit fix` for auto-fixable dependency issues
     - Remove or rotate any exposed credentials
     - Add input validation where missing
     - Add authorization checks where missing
   - Document any issues that require manual intervention

9. **Security Headers & Best Practices** (if web app)
   - Check for proper CORS configuration
   - Verify Content Security Policy headers
   - Look for missing rate limiting on sensitive endpoints

10. **Report & Summary**
    - Summarize all findings by severity (Critical, High, Medium, Low)
    - List fixes applied automatically
    - List issues requiring manual review
    - Suggest next steps for ongoing security hygiene

## Notes
- Never commit fixes that might break functionality without running tests first
- If credentials are found in git history, note that history rewrite may be needed
- For critical vulnerabilities, flag immediately before proceeding with other steps
- Use `npm audit --audit-level=moderate` to focus on actionable issues
