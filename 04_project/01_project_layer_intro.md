# Directory Purpose

This layer is meant to dictate specifics on how to leverage different technologies/stacks, and overlap or combinations between them within this specific project. It may make note that you're only using specific parts of a stack, or choosing one data-flow over another, etc. It's meant to be gradually more specific than outlining how to use a stack in general. 

This is where one would generally provide a master brief/description of the project, an architecture document, database-design document, and long-lived context that's important for the lifespan of the project.

Here's a simplified example of a single/starter document (note: the example below isn't meant to be instructions, it's meant to serve as an example as to how to format documents in this directory):
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