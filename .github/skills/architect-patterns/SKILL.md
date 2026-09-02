# Architect Patterns — System Design SKILL

This skill provides **architectural decision templates, system design patterns, and tradeoff frameworks** for designing features.

## When to Use This Skill

- Designing new features with infrastructure/data flow requirements
- Validating technical feasibility before implementation
- Documenting architectural decisions for a feature
- Discussing tradeoffs (latency vs cost, consistency vs availability)

---

## System Design Principles

### 1. Define the Contract Before Implementation
Architect's job: specify WHAT (data model, API boundary, external services, scaling targets).
Developer's job: implement HOW (code, optimization, testing).

### 2. Data-Driven Design
Identify:
- Primary data store (database, cache, file store)
- Access patterns (reads vs writes, frequency, scale)
- Consistency model (strong vs eventual, CAP tradeoffs)

### 3. Bounded Domains
Keep services loosely coupled:
- Clear API boundaries
- Minimal shared state
- Independent deployment (if applicable)

### 4. Tradeoffs Are Explicit
Every architectural decision has a tradeoff. State it clearly:
- **Choice:** Async email delivery
- **Pro:** User gets response fast (< 100ms)
- **Con:** Email delivery is eventual consistent (may take seconds)
- **Recommendation:** Accept async, use queues (SQS, etc.)

---

## Architectural Decision Template

Use this template when a feature requires significant architectural decisions:

```
# Feature: [Title]

## Problem Statement
What user/system problem are we solving?

## Proposed Architecture

### Data Model
- Primary store: [database | cache | file store]
- Schema/structure: [exact definition]
- Access patterns: [how data is read/written]
- Scaling: [expected scale, growth path]
- Consistency: [strong | eventual | CQRS]

### API Boundaries
- Route/endpoint: [POST /api/feature, etc.]
- Auth: [public | authenticated | server-to-server]
- Payload: [request/response schema]
- Error handling: [what can fail, recovery]

### External Services
- [Service name]: [why, cost, scale considerations]
- [Service name]: [why, cost, scale considerations]

### Deployment
- Single service | microservices | serverless
- Infrastructure: [CDK, Terraform, etc.]
- Operational burden: [monitoring, scaling, backups]

## Tradeoffs

| Decision | Pro | Con | Recommendation |
|----------|-----|-----|---|
| Async email | Fast response to user | Eventual consistency | ✅ Accept |
| DynamoDB | Scales easily | No joins (data duplication) | ✅ Accept |
| S3 for content | Cheap, CDN-friendly | No versioning/transactions | ✅ Accept |

## Risks

- [Risk]: [Mitigation]
- [Risk]: [Mitigation]

## Technical Acceptance Criteria

- [ ] Data model supports all access patterns
- [ ] Scaling path documented (x10, x100 growth)
- [ ] No single point of failure (HA/DR plan)
- [ ] Estimated cost is acceptable
- [ ] Deployment process is documented
```

---

## Common Patterns

### Server-Fetched vs Client-Fetched

| Pattern | When to Use | Tradeoff |
|---------|-------------|----------|
| **Server-Fetched (Server Component / SSR)** | Static content, SEO, security-sensitive | Initial load slower, more server load |
| **Client-Fetched (SPA / Client Component)** | Interactive, user-specific, real-time | More JS, harder to secure, SEO challenges |

**Recommendation:** Default to server-fetched. Client-fetch only when you need interactivity.

### Route Handler API Pattern

```typescript
// POST /api/feature
export async function POST(request: Request) {
  const body = await request.json();
  
  // 1. Validate input
  if (!isValid(body)) return error(400, 'Invalid input');
  
  // 2. Check auth
  const user = await getCurrentUser();
  if (!user) return error(401, 'Unauthorized');
  
  // 3. Execute business logic
  const result = await createFeature(body, user);
  
  // 4. Persist to database
  await db.save(result);
  
  // 5. Return response
  return success(201, result);
}
```

### Data Modeling (Example: DynamoDB)

```
Table: subscribers

Partition Key (PK): email (string)
Sort Key (SK): —
TTL: expiresAt (timestamp)

Attributes:
  email (PK)
  status: pending | active | bounced
  createdAt: timestamp
  verificationToken: string (encrypted)
  verificationTokenExpires: timestamp

GSI 1 (status-createdAt-index):
  PK: status
  SK: createdAt
  → Allows querying: "all active subscribers by date"

Access Patterns:
  1. Get subscriber by email → Query by PK
  2. Get all active subscribers → Query GSI1 where status='active'
  3. Expire unverified → Scan and delete where TTL < now
```

---

## Validation Checklist

Before handing off to Security/Designer:

- [ ] Data model defined (schema, keys, indexes)
- [ ] API boundaries clear (endpoints, payloads, auth)
- [ ] External services identified (and costs estimated)
- [ ] Scaling path documented (10x, 100x growth)
- [ ] Consistency model stated (strong | eventual)
- [ ] Operational concerns noted (monitoring, backups, disasters)
- [ ] Tradeoffs documented and approved
- [ ] No undocumented assumptions

---

## Anti-Patterns

❌ **Over-engineering** — designing for 1000x scale on day 1  
✅ **Better:** Design for current scale + growth path

❌ **Too many external services** — every feature needs a new API  
✅ **Better:** Consolidate, minimize operational burden

❌ **Consistency everywhere** — strong consistency on all data  
✅ **Better:** Know your consistency model per data type

❌ **Vague API contracts** — "users can do things"  
✅ **Better:** Exact endpoint, exact payload, exact error codes

---

## References

- **Data Modeling:** [Your stack docs](https://example.com)
- **API Design:** [Your API guidelines](https://example.com)
- **Infrastructure:** [Your CDK / Terraform docs](https://example.com)
