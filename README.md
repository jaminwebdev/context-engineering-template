## Tier Structure
```
┌─────────────────────────────────────┐
│   Base Layer (Always Active)        │  ~500-1000 tokens
│   - Communication style             │
│   - General principles              │
│   - Output preferences              │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Domain Layer (Conditional)         │  ~1000-2000 tokens
│   - Programming rules                │
│   - Business analysis framework      │
│   - Research methodology             │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Stack Layer (Conditional)          │  ~2000-4000 tokens
│   - SvelteKit architecture           │
│   - NextJS patterns                  │
│   - Convex + Svelte integration      │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Project Layer (Specific)           │  ~1000-3000 tokens
│   - Current schemas                  │
│   - Active components                │
│   - Project-specific rules           │
└─────────────────────────────────────┘
```

## Domain Layer Breakdown

### Example Base Layer Document: - `01_base/core-instructions.md`

This is your personality and universal preferences:

```markdown
# Core Instructions

## Communication Style
- Be concise but thorough
- Ask clarifying questions before making assumptions
- Provide reasoning for architectural decisions
- Challenge my ideas constructively when warranted

## Code Principles
- Prioritize readability over cleverness
- Favor composition over inheritance
- Write self-documenting code with strategic comments
- Always consider error states and edge cases

## Default Behaviors
- For any database operation: include timestamps and userId
- For any form: validate on client and server
- For any component: consider loading and error states
- For any API: think about rate limiting and auth

## Output Preferences
- Don't apologize unnecessarily
- Don't repeat my question back to me
- Provide working code, not pseudocode
- Include type definitions when relevant
```

### Example Domain Layer: `02_domain/`

Create separate files for each major domain:

**`programming.md`:**

```markdown
# Programming Domain Rules

## General Approach
- Start with the simplest solution that works
- Optimize only when there's measured need
- Prefer boring, proven technology over shiny new tools
- Progressive enhancement over degradation

## Code Organization
- Colocate related code (features, not file types)
- Keep functions small and single-purpose
- Extract shared logic after 3rd duplication, not before
- Use consistent naming conventions within a project

## Testing Philosophy
- Write tests for complex business logic
- Don't test implementation details
- Integration tests > unit tests for web apps
- Test user flows, not internal functions

## When I Say "Make it production ready"
1. Add proper error handling and user feedback
2. Include loading states
3. Add input validation
4. Consider edge cases
5. Add appropriate logging
6. Think about accessibility
7. Consider mobile responsiveness
```

**`business-analysis.md`:**

```markdown
# Business Analysis Framework

## When Analyzing Client Operations
1. Start with current pain points (don't solution first)
2. Quantify impact (time saved, error reduction, revenue impact)
3. Consider change management and adoption barriers
4. Provide phased implementation options
5. Always include ROI estimates

## SOP Audit Process
1. Map current workflow end-to-end
2. Identify bottlenecks and manual steps
3. Flag inconsistencies and error-prone areas
4. Prioritize by impact × ease matrix
5. Suggest automation opportunities
6. Document assumptions clearly

## Pricing Model Approach
- Value-based pricing over hourly when possible
- Consider client's ROI, not just my time
- Build in scope flexibility buffers
- Tiered options (good/better/best)
```


### Example Stack Layer Document: `03_stack/svelte_rules.md`

**`svelte_rules.md`:**

````markdown
# SvelteKit Architecture Preferences

## Data Flow (CRITICAL - READ FIRST)

### Preferred Patterns (Use These for RESTFUL/non-Convex powered back ends)
✅ Load data in +page.server.ts load functions
✅ Mutations via Form Actions in +page.server.ts
✅ Progressive enhancement with use:enhance
✅ Server-side validation before client-side

### Anti-Patterns (Avoid These)
❌ NO standalone API endpoints unless for external consumption
❌ NO fetch() calls from client to your own API routes
❌ NO client-side only data fetching
❌ NO putting logic in +page.ts that could be in +page.server.ts

## Rationale
- Form Actions provide built-in CSRF protection
- Server-side rendering improves SEO and initial load
- Progressive enhancement ensures functionality without JS
- Simpler mental model (no API contract to maintain)

## The Right Way™

### Loading Data
```typescript
// +page.server.ts
export const load = async ({ locals, params }) => {
  const data = await db.query.users.findMany({
    where: eq(users.organizationId, locals.user.orgId)
  });
  
  return { users: data };
};
```

### Mutations
```typescript
// +page.server.ts
export const actions = {
  create: async ({ request, locals }) => {
    const formData = await request.formData();
    const result = await createSchema.safeParseAsync(
      Object.fromEntries(formData)
    );
    
    if (!result.success) {
      return fail(400, { 
        errors: result.error.flatten().fieldErrors 
      });
    }
    
    await db.insert(users).values({
      ...result.data,
      userId: locals.user.id,
      createdAt: new Date()
    });
    
    return { success: true };
  }
};
```

## Package Integration Rules

### superForms
- ALWAYS use dataType: 'json' in superForm() options
- Server-side validation is source of truth


### Auth (Lucia/Better-Auth)
- Session validation in hooks.server.ts
- Protect server routes, not client routes
- Store minimal data in session token
````
### Example Project Layer: `04_project/current-project.md`

**`current-project.md`** (you swap this out as needed):

markdown

````markdown
# Project: ClientPortal

## Tech Stack
- SvelteKit + TypeScript
- Convex (database + real-time)
- Stripe (payments)
- Resend (emails)

## Database Schema (Key Tables)
```typescript
// Relevant schema snippets
organizations { id, name, plan, createdAt }
users { id, email, orgId, role, createdAt }
documents { id, title, content, orgId, userId, createdAt, updatedAt }
```

## Current Architecture Decisions
- Multi-tenant by organizationId (row-level filtering)
- Role-based access: admin, member, viewer
- All mutations check org membership first
- File uploads go to Convex storage

## Active Components/Patterns in Use
- DocumentEditor: Tiptap-based editor with auto-save
- OrgSwitcher: Dropdown for users in multiple orgs
- BillingPortal: Stripe customer portal integration

## Known Issues/Quirks
- Convex file URLs expire, refresh on load
- Org switching requires full page reload (session change)
- Heavy documents (>100KB) should paginate

## TODO / Upcoming
- Add document sharing
- Implement commenting system
- Upgrade to team-based pricing
````

## Template for New Documents

Every context document should have:

```markdown
# [Topic Name]

## When to Use This Context
[Describe scenarios where this should be included]

## Key Principles
[3-5 core rules or preferences]

## Patterns to Follow
[Concrete examples with code/structure]

## Anti-Patterns
[What NOT to do - this is crucial!]

## Integration Notes
[How this works with other parts of stack]

## Examples
[Working code snippets]
```
