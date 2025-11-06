## Tier Structure
```
┌─────────────────────────────────────┐
│   Base Layer (Always Active)        │
│   - Communication style             │
│   - General principles              │
│   - Output preferences              │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Domain Layer (Conditional)         │
│   - Programming rules                │
│   - Business analysis framework      │
│   - Research methodology             │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Stack Layer (Conditional)          │
│   - SvelteKit architecture           │
│   - NextJS patterns                  │
│   - Convex + Svelte integration      │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Project Layer (Specific)           │
│   - Current schemas                  │
│   - Active components                │
│   - Project-specific rules           │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Example Layer (Specific)           │
│   - Working examples                 │
│   - Anti-patterns                    │
│   - Explanations for one-offs        │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Memory Layer (Specific)            │
│   - Active & active session notes    │
│   - Architectural decisions over time│
└─────────────────────────────────────┘
```

## Domain Layer Breakdown

See the `01_{layer}_intro.md` for each directory for the purpose of said directory and an example of how to structure documents. 

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
