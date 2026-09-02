# Security Checklist — Threat Modeling SKILL

This skill provides **threat modeling templates, auth patterns, encryption guidance, and compliance frameworks** for designing secure features.

## When to Use This Skill

- Designing features with user data, auth, or external APIs
- Threat-modeling a new API endpoint
- Reviewing security requirements from architecture/design
- Compliance concerns (GDPR, HIPAA, etc.)

---

## Threat Model Template (4-Step)

### 1. Identify Assets
What are we protecting?

```
Assets for subscriber feature:
- User email (PII, identifiable)
- Mailing list (business asset)
- Verification token (authentication credential)
- Subscriber status (personal data)
```

### 2. Map Attack Surface
How could attackers access these assets?

```
Attack Surface:
- Public POST /api/subscribe endpoint (no auth)
- Email field in request body (user input)
- Verification link in email (if leaked, can confirm account)
- Database connection (if compromised, all emails exposed)
- Email service credentials (if leaked, can send emails as us)
```

### 3. Enumerate Scenarios
What attacks are realistic?

```
Scenarios:
1. Email Injection
   Attacker: provides email = "user@example.com\nbcc: attacker@evil.com"
   Impact: Attacker BCC'd on confirmation emails
   Likelihood: Medium (common mistake)
   
2. Rate Limiting Bypass
   Attacker: makes 1000 requests per second
   Impact: System overloaded, DoS
   Likelihood: High (easy to exploit)
   
3. Token Prediction
   Attacker: verification token is "123456" (predictable)
   Impact: Attacker confirms any email
   Likelihood: High (weak randomness)
   
4. CSRF Attack
   Attacker: tricks user into clicking malicious link
   Impact: User unsubscribes without knowing
   Likelihood: Medium (common web attack)
```

### 4. Define Mitigations
How do we protect?

```
Mitigations:
1. Email Injection
   → Validate email format (RFC 5322)
   → Use dedicated email service (SES, SendGrid)
   → Never interpolate user email into headers
   
2. Rate Limiting
   → Enforce 10 requests/IP/minute at API Gateway
   → Log and alert on suspicious patterns
   → Optional: CAPTCHA after N attempts
   
3. Token Prediction
   → Use crypto.randomUUID() (not Math.random())
   → Token must be 256 bits of entropy minimum
   → Token expires after 24h
   
4. CSRF Attack
   → Verify CSRF token in request
   → Use SameSite=Strict cookies
   → Confirmation link is one-time-use
```

---

## Auth Patterns

### Pattern 1: Public Endpoint (No Auth)

```
Use when: anyone can access (e.g., subscribe form)

Risk: Must validate input carefully (email injection, XSS)

Mitigations:
- Rate limit by IP
- Validate input strictly
- Never trust user input in SQL/emails/headers
- Log all requests
```

### Pattern 2: Authenticated Endpoint (Auth Required)

```
Use when: only logged-in users access (e.g., view profile)

Risk: Token leakage (JWT, session cookie theft)

Mitigations:
- Use httpOnly, SameSite=Strict cookies
- Short token expiration (15-60 min)
- Refresh tokens (longer expiration)
- Validate token on every request
- Revoke token on logout/password change
```

### Pattern 3: Server-to-Server (Service Account)

```
Use when: one service calls another (e.g., Lambda → DynamoDB)

Risk: Credential exposure, overly broad permissions

Mitigations:
- Use IAM roles (AWS) or service accounts (GCP) — never hardcoded keys
- Least-privilege permissions (only what's needed)
- Rotate credentials regularly
- Log all calls (who called whom, when, what)
```

---

## Encryption Patterns

### In Transit (TLS)

```
Rule: All data in motion must be encrypted

Implementation:
- Use HTTPS (TLS 1.3 minimum)
- Redirect HTTP → HTTPS
- HSTS header (enforce HTTPS for 1 year)
- Certificate pinning (optional, for mobile apps)
```

### At Rest (Database/File Storage)

```
Rule: Sensitive data must be encrypted in database

Implementation:
- Enable database encryption (AWS KMS, Azure Key Vault)
- Encrypt sensitive fields (email, PII, tokens)
- Never store passwords in plaintext (hash with bcrypt/Argon2)
- Never store API keys in database (use secrets manager)
```

### Secrets Management

```
Rule: Never commit credentials to version control

What needs protecting:
- API keys (SES, Stripe, third-party APIs)
- Database passwords
- Webhook signing keys
- Encryption keys

Where to store:
- Environment variables (dev)
- Secrets manager (AWS Secrets Manager, GitHub Secrets)
- Hardware security modules (production, if required)

Rotation:
- Rotate credentials quarterly
- Automated rotation for database passwords
- Immediate rotation if leaked
```

---

## Validation Patterns

### Input Validation

```typescript
// Email validation
const isValidEmail = (email: string) => {
  // RFC 5322 minimal check
  const pattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return pattern.test(email) && email.length < 254;
};

// Never interpolate user input into:
// - SQL queries (use parameterized queries)
// - Email headers (use email service API)
// - HTML/JavaScript (use DOMPurify or React escaping)
```

### SQL Injection Prevention

```typescript
// ❌ VULNERABLE
const query = `SELECT * FROM users WHERE email = '${email}'`;

// ✅ SAFE (parameterized query)
const query = 'SELECT * FROM users WHERE email = ?';
const result = db.query(query, [email]);
```

### XSS Prevention

```typescript
// ❌ VULNERABLE
return `<h1>${userEmail}</h1>`;

// ✅ SAFE (React escapes by default)
return <h1>{userEmail}</h1>;
```

---

## Compliance Patterns

### GDPR (EU User Data)

```
Requirements:
- [ ] Users can request their data (export)
- [ ] Users can delete their account (right to be forgotten)
- [ ] Data is only used for stated purpose
- [ ] Data is retained only as long as needed
- [ ] Privacy policy is clear and accessible
- [ ] Consent is explicit (not pre-checked)

Implementation:
- Add DELETE /api/users/{id} endpoint
- Add GET /api/users/{id}/data endpoint (returns JSON)
- Set data retention policy (e.g., 2 years)
- Document data flows in privacy policy
```

### Data Retention

```
Define how long you keep data:

- Active subscriber: keep email + status indefinitely
- Unconfirmed subscriber: delete after 30 days
- Deleted user: GDPR right-to-be-forgotten (instant)
- Audit logs: keep for 1 year (compliance)
- Failed login attempts: keep for 90 days (security)
```

---

## Security Acceptance Criteria

Define testable security requirements:

```markdown
- [ ] Email is validated (RFC 5322 + length < 254)
- [ ] Email is never sent unescaped (XSS prevention)
- [ ] Verification token is cryptographically random (crypto.randomUUID())
- [ ] Verification link is one-time-use (marked as "used" after click)
- [ ] Rate limit is enforced (10 requests/IP/minute)
- [ ] Unconfirmed emails never sent to mailing list
- [ ] Unconfirmed subscribers auto-delete after 30 days
- [ ] Secrets (SES credentials) not in code or logs
- [ ] All API responses are HTTPS only
- [ ] CSRF token required for form submission
```

---

## Security Review Checklist

Before handing off to Design:

- [ ] Threat model complete (assets, surface, scenarios, mitigations)
- [ ] Auth pattern chosen (public | authenticated | service-to-server)
- [ ] Encryption requirements defined (in transit, at rest, secrets)
- [ ] Input validation plan (email, rates, injection prevention)
- [ ] Compliance requirements identified (GDPR, etc.)
- [ ] Security AC written (testable requirements)
- [ ] No undiscovered security gaps
- [ ] Team agreed on mitigations

---

## Anti-Patterns

❌ **Security by obscurity** — hiding things instead of protecting them  
✅ **Better:** Assume attacker knows everything; protect through encryption/validation

❌ **Credentials in code** — hardcoded API keys  
✅ **Better:** Secrets manager (AWS Secrets Manager, GitHub Secrets)

❌ **No rate limiting** — anyone can spam  
✅ **Better:** Rate limit by IP, user, API key

❌ **Weak randomness** — Math.random() for tokens  
✅ **Better:** crypto.randomUUID() or crypto.getRandomValues()

❌ **No data retention policy** — keep everything forever  
✅ **Better:** Define retention (e.g., 2 years) + auto-delete

---

## References

- **OWASP Top 10:** [https://owasp.org/www-project-top-ten](https://owasp.org/www-project-top-ten)
- **OWASP Cheat Sheets:** [https://cheatsheetseries.owasp.org](https://cheatsheetseries.owasp.org)
- **GDPR Guide:** [https://gdpr-info.eu](https://gdpr-info.eu)
- **TLS Best Practices:** [https://wiki.mozilla.org/Security/Server_Side_TLS](https://wiki.mozilla.org/Security/Server_Side_TLS)
