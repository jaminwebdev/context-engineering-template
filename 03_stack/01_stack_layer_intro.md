# Directory Purpose

This layer is meant to dictate specifics on how to leverage different technologies/stacks, and overlap or combinations between them - such as how to use frameworks like Svelte and SvelteKit or NextJS. 

Here's a simplified example document (note: the example below isn't meant to be instructions, it's meant to serve as an example as to how to format documents in this directory):
````markdown
# SvelteKit Architecture Preferences

## Data Flow (CRITICAL - READ FIRST)

### Preferred Patterns (Use These)
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
```