# Testing Strategy — Test Pyramid SKILL

This skill provides **test strategy frameworks, pragmatic test patterns, and AC → test mapping** aligned to the test pyramid.

## When to Use This Skill

- Designing test strategy for a feature
- Mapping acceptance criteria to tests
- Defining critical paths and edge cases
- Validating testability of design/architecture

---

## Test Pyramid

```
          /\
         /  \  E2E (10%)
        /    \
       /______\
      /        \
     / Integration\  (25%)
    /            \
   /______________\
  /                \
 /      Unit        \ (60%)
/                    \
______________________
```

**Allocation:**
- **60% Unit Tests** — fast, isolated, business logic
- **25% Integration Tests** — API routes, database, external services
- **10% E2E Tests** — user journeys, critical paths only

**Why this ratio?**
- Unit tests are cheap (fast, no setup)
- Integration tests are moderate (some setup, slower)
- E2E tests are expensive (slow, flaky, hard to debug)

---

## AC → Test Mapping Template

For each acceptance criterion, define the test type and implementation:

```markdown
| AC | Unit Test | Integration Test | E2E Test |
|----|-----------|--------------------|----------|
| User can enter email | isValidEmail('test@example.com') === true | N/A | N/A |
| Form submission works | N/A | POST /api/subscribe → status 200 | User fills form → success message |
| Email is sent | N/A | SES queue receives email + token | (Simulated) Email link works |
| Unverified emails excluded | N/A | Query where status='pending' → never sent | N/A |
| Rate limit enforced | N/A | 11 requests per IP → status 429 | N/A |
```

---

## Unit Test Patterns

**Goal:** Test business logic in isolation. No dependencies, no side effects.

### Example: Email Validation

```typescript
import { describe, it, expect } from 'vitest';
import { isValidEmail } from '@/lib/validators';

describe('isValidEmail', () => {
  it('accepts valid emails', () => {
    expect(isValidEmail('user@example.com')).toBe(true);
    expect(isValidEmail('test+tag@example.co.uk')).toBe(true);
  });

  it('rejects invalid formats', () => {
    expect(isValidEmail('invalid')).toBe(false);
    expect(isValidEmail('@example.com')).toBe(false);
    expect(isValidEmail('user@')).toBe(false);
  });

  it('rejects dangerous patterns', () => {
    expect(isValidEmail('user+`<img>`@example.com')).toBe(false);
  });
});
```

### Best Practices

- **One assertion per happy path** — test one behavior
- **Multiple assertions in error cases** — group related errors
- **Use parameterized tests** — test multiple inputs efficiently
- **Name tests like stories** — describe what succeeds/fails

---

## Integration Test Patterns

**Goal:** Test API routes, database operations, and service interactions. Use mocks or test services (LocalStack, test database).

### Example: Subscribe Endpoint

```typescript
import { describe, it, expect, beforeAll } from 'vitest';
import { POST } from '@/app/api/subscribe/route';

describe('POST /api/subscribe', () => {
  it('creates a pending subscriber', async () => {
    const request = createRequest({
      body: { email: 'user@example.com' }
    });

    const response = await POST(request);
    
    expect(response.status).toBe(200);
    
    const subscriber = await db.subscribers.get('user@example.com');
    expect(subscriber.status).toBe('pending');
    expect(subscriber.verificationToken).toBeDefined();
  });

  it('sends verification email', async () => {
    const request = createRequest({
      body: { email: 'user@example.com' }
    });

    await POST(request);
    
    const emails = await ses.listSentEmails();
    expect(emails).toContainEqual({
      to: 'user@example.com',
      subject: 'Verify your subscription'
    });
  });

  it('rejects duplicate subscriptions', async () => {
    // First subscription
    await POST(createRequest({ body: { email: 'user@example.com' } }));
    
    // Second subscription with same email
    const response = await POST(createRequest({ body: { email: 'user@example.com' } }));
    
    expect(response.status).toBe(409); // Conflict
  });

  it('rate limits by IP', async () => {
    const ip = '192.168.1.1';
    
    // Make 10 requests (within limit)
    for (let i = 0; i < 10; i++) {
      const response = await POST(createRequest(...), { clientIp: ip });
      expect(response.status).toBe(200);
    }
    
    // 11th request exceeds limit
    const response = await POST(createRequest(...), { clientIp: ip });
    expect(response.status).toBe(429); // Too Many Requests
  });
});
```

---

## E2E Test Patterns

**Goal:** Test user journeys. Only test critical paths. Use Playwright or similar.

### Example: Subscribe + Confirm Flow

```typescript
import { test, expect } from '@playwright/test';

test('user can subscribe and confirm', async ({ page }) => {
  // 1. Navigate to subscribe form
  await page.goto('/');
  
  // 2. Fill and submit form
  await page.fill('input[type=email]', 'user@example.com');
  await page.click('button[type=submit]');
  
  // 3. Success message appears
  await expect(page.locator('text=Check your email')).toBeVisible();
  
  // 4. (Simulated) Extract verification link from email
  const verificationLink = await getVerificationLink('user@example.com');
  
  // 5. Click link to confirm
  await page.goto(verificationLink);
  
  // 6. Verify success
  await expect(page.locator('text=Subscription confirmed')).toBeVisible();
  
  // 7. Check database
  const subscriber = await db.subscribers.get('user@example.com');
  expect(subscriber.status).toBe('active');
});
```

### Best Practices

- **Critical paths only** — one happy path, maybe one error path
- **Semantic locators** — use `role`, `label`, `text` instead of CSS selectors
- **Wait for state** — use `expect().toBeVisible()` not `await page.waitForTimeout()`
- **Simulate real behavior** — fill forms like users do, click buttons, navigate

---

## QE Verification Tests

**Who:** QE owns these tests. Developers implement them.  
**What:** Tests that verify implementation details QE cares about but developers might skip.  
**Why:** Ensures code quality, security, compliance beyond AC.

### Example QE Verification Tests

For subscribe feature:

- ✅ Unconfirmed emails are NEVER sent to mailing list (security)
- ✅ Verification token is cryptographically random (security)
- ✅ Token expires after 24h (compliance)
- ✅ Duplicate emails are detected before DB write (performance)
- ✅ Rate limit is enforced at infrastructure level, not app level (scale)
- ✅ XSS prevention: user email is never stored/displayed unescaped (security)

These tests document what "correct" means beyond the AC.

---

## Test AC (Acceptance Criteria for Tests)

Define what makes a test "good":

- [ ] Each test has at least one business-rule assertion
- [ ] No test is flaky (passes/fails consistently)
- [ ] All critical paths have E2E tests
- [ ] All edge cases have unit or integration tests
- [ ] Test names describe what succeeds/fails
- [ ] Setup/teardown is fast (< 1s per test)
- [ ] Tests are independent (can run in any order)

---

## Validation Checklist

Before handing off to developers:

- [ ] Test pyramid defined (60/25/10 allocation)
- [ ] AC → test mapping complete
- [ ] Critical paths identified
- [ ] Unit tests specified (with examples)
- [ ] Integration tests specified (with examples)
- [ ] E2E tests specified (user journeys)
- [ ] QE verification tests identified (QE concerns)
- [ ] Test AC clear (what makes a test valid)
- [ ] No undiscovered test gaps

---

## Anti-Patterns

❌ **100% coverage obsession** — tests everything, but slow and brittle  
✅ **Better:** Focus on business logic and critical paths

❌ **E2E tests for everything** — slow, flaky, hard to debug  
✅ **Better:** Unit test logic, E2E test user journeys

❌ **Mocks everywhere** — tests pass, but reality fails  
✅ **Better:** Integration tests with real services (test DB, LocalStack)

❌ **No QE verification tests** — developers implement unsafely  
✅ **Better:** QE specifies additional correctness tests

---

## References

- **Vitest Docs:** [https://vitest.dev](https://vitest.dev)
- **Playwright Docs:** [https://playwright.dev](https://playwright.dev)
- **Test Pyramid:** Martin Fowler's classic post
