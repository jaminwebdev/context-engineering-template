# Directory Purpose

This layer is meant to act as ongoing memory management between sessions and LLM models. Said memory dumps are meant to be as minimal as needed to keep context brief and low, but with enough detail to pick up where you left off and provide a more complete history than something like version control (Git) could provide. 

This will require ongoing maintenance and oversight, as well as correcting or maintaining references to other documents throughout. The archive directory in particular is meant to be the last resort for remembering what was done or completed. 

Note: Architectural and project specific decisions and changes may require updating core documentation outside of this directory. It should be done to keep everything in sync, and be done at the end of a session as noted below. 

Crucially, these items should be recorded at the end of sessions, not be built throughout the course of a session - that would rapidly eat up context and become a nightmare to maintain. 

Here's how the directory is structured and should work:

## The Three Memory Types

### 1. Active Session Threads (`06_memory/active/`)

These are living documents for ongoing work. One file per feature/problem.

**Template: `feature-auth-improvements.md`**

````markdown
# Feature: Auth Improvements
**Status**: In Progress
**Started**: 2024-11-05
**Last Updated**: 2024-11-06 14:30
**Files Modified**: src/routes/login/+page.server.ts, src/lib/server/auth.ts

---

## Problem Statement
Users can't reset passwords. Current auth flow lacks email verification.

## Goals
- [ ] Implement password reset flow
- [ ] Add email verification
- [x] Fix session expiration bug
- [ ] Add rate limiting to login

---

## Session 1: 2024-11-05 Morning
**What we did**: Fixed session expiration bug

**The Issue**: 
Sessions were expiring immediately due to cookie settings conflicting with Vercel deployment.

**Solution**:
Changed cookie settings in `src/lib/server/auth.ts`:
```typescript
// Before
cookie: { secure: true, sameSite: 'strict' }

// After  
cookie: { 
  secure: process.env.NODE_ENV === 'production',
  sameSite: 'lax'  // Changed from strict
}
```

**Key Learning**: 
Vercel preview deployments aren't served over localhost, so `secure: true` breaks them. Use environment-based logic.

**Next Session**: Start on password reset flow.

---

## Session 2: 2024-11-06 Afternoon
**What we did**: Designed password reset flow

**Decisions Made**:
1. Use time-limited tokens (1 hour expiry)
2. Store reset tokens in `password_reset_tokens` table
3. Send emails via Resend (already integrated)
4. Invalidate token after use

**Schema Added**:
```typescript
export const passwordResetTokens = pgTable('password_reset_tokens', {
  id: uuid('id').defaultRandom().primaryKey(),
  userId: uuid('user_id').references(() => users.id).notNull(),
  token: text('token').notNull().unique(),
  expiresAt: timestamp('expires_at').notNull(),
  createdAt: timestamp('created_at').defaultNow().notNull()
});
```

**Files Created**:
- `src/routes/reset-password/+page.svelte` (request form)
- `src/routes/reset-password/+page.server.ts` (send email action)
- `src/routes/reset-password/[token]/+page.svelte` (reset form)
- `src/routes/reset-password/[token]/+page.server.ts` (validate & update)

**Implementation Notes**:
- Token is crypto.randomBytes(32).toString('hex')
- Email template is in `src/lib/emails/password-reset.ts`
- Rate limit: 3 requests per hour per email

**Gotchas Discovered**:
- Must hash token before storing (don't store plaintext)
- Clean up expired tokens with a cron job (added to TODO)
- Resend requires "from" address to be verified domain

**Blockers**: 
None currently.

**Next Session**: 
Implement the actual password reset routes and test flow end-to-end.

---

## Session 3: [Next time...]

[Leave this empty - fill in next session]

---

## Reference Links
- Lucia Auth docs: https://lucia-auth.com/guides/password-reset
- Similar implementation in old project: `~/projects/archive/crm-app/auth-reset.md`

## Related Context
When loading next session, include:
- Stack: sveltekit-architecture, lucia-auth
- Examples: email-sending-pattern
````

### 2. Decision Log (`06_memory/decisions/`)

**Architectural decisions that affect multiple sessions.**

**Template: `decisions.md`**

```markdown
# Architectural Decisions Log

## Format
Each decision includes: Context, Decision, Rationale, Consequences, Date

---

## ADR-001: Use Form Actions Over API Endpoints (2024-10-15)

**Context**: 
Building client portal with forms for CRUD operations. Could use API routes or Form Actions.

**Decision**: 
Use SvelteKit Form Actions for all mutations, avoid standalone API endpoints.

**Rationale**:
- Built-in CSRF protection
- Progressive enhancement (works without JS)
- Simpler mental model (no API contract)
- Better TypeScript integration
- Easier to colocate with page logic

**Consequences**:
- Mobile app would need separate GraphQL/REST layer
- External integrations need dedicated API routes
- Some client-side validation duplication

**Status**: Active
**Referenced In**: sveltekit-architecture.md

---

## ADR-002: Multi-tenancy via Row-Level Filtering (2024-10-20)

**Context**:
App serves multiple organizations. Could use separate databases, schemas, or row-level filtering.

**Decision**:
Single database with `organizationId` on all tables. Filter in application code.

**Rationale**:
- Simpler deployment (one database)
- Easier cross-org analytics
- Lower operational overhead
- Drizzle RLS helpers make it safe

**Consequences**:
- Must remember to filter EVERY query
- One bad query could leak data across orgs
- Harder to isolate org for data export

**Mitigations**:
- Created `withOrgFilter()` helper (wraps queries)
- All DB operations go through typed functions
- Added tests for org isolation

**Status**: Active
**Code Location**: `src/lib/server/db/filters.ts`

---

## ADR-003: Convex for Real-Time, Postgres for Relational (2024-11-01)

**Context**:
Need real-time collaboration features but also complex relational queries.

**Decision**:
Hybrid: Convex for documents/real-time, Postgres for users/orgs/billing.

**Rationale**:
- Convex excels at real-time subscriptions
- Postgres better for transactions and complex joins
- Each tool optimized for its use case

**Consequences**:
- Data split across two systems
- Need sync mechanism for user data
- More complex deployment

**Trade-offs Accepted**:
- Worth the complexity for real-time UX
- Keep sync minimal (only user metadata)

**Status**: Active - monitoring complexity
**Review Date**: 2024-12-01

---

## Template for New Decisions

## ADR-XXX: [Decision Title] (YYYY-MM-DD)

**Context**: 
[What problem are we solving? What constraints exist?]

**Decision**: 
[What did we decide to do?]

**Rationale**:
[Why this approach over alternatives?]

**Consequences**:
[What are the trade-offs? What new constraints does this create?]

**Status**: [Active | Superseded | Deprecated]
**Superseded By**: [If deprecated, link to new decision]
```

### 3. Archive (`06_memory/archive/`)

**Completed work for reference.**

When a feature is done, move from `active/` to `archive/` and add a summary:

```markdown
# [COMPLETED] Feature: Auth Improvements
**Completed**: 2024-11-08
**Total Sessions**: 4
**Files Modified**: 12 files
**Lines Changed**: +487, -124

## Final Outcome
✅ Password reset flow working
✅ Email verification implemented  
✅ Rate limiting added
✅ Session bug fixed

## What Worked Well
- Breaking into small sessions kept progress steady
- Session notes prevented re-discovery of issues
- Decision log avoided revisiting solved problems

## What Could Be Better
- Should have added integration tests from start
- Rate limiting was afterthought (should be earlier)

## Key Files
- `src/routes/reset-password/**/*` (password reset)
- `src/lib/server/auth.ts` (core auth logic)
- `src/lib/emails/` (email templates)

## Lessons for Next Time
- Hashing tokens: use bcrypt, not crypto.createHash
- Resend requires verified domain (test with dev key first)
- Cookie settings matter more than expected

## Archive Note
If implementing similar auth flow, reference:
- Session 2 for schema design
- Session 3 for token security pattern
- Decision ADR-004 for email provider choice
```

### What to Capture

**Essential:**

- ✅ The actual problem (not just "fix auth")
- ✅ Key decisions and WHY
- ✅ Files created/modified
- ✅ Code patterns that worked
- ✅ Gotchas/surprises/bugs discovered
- ✅ What to do next

**Skip:**

- ❌ Full code dumps (reference files instead)
- ❌ Conversation transcript
- ❌ Stuff already in docs
- ❌ Obvious next steps