# Directory Purpose

This layer is meant to add crucial boilerplate that doesn't fit well into other documents (such as any architectural docs or domain/stack level docs) as to not overcrowd them. 

Here's an example document (note: the example below isn't meant to be instructions, it's meant to serve as an example as to how to format documents in this directory):
**`sveltekit-crud-pattern.md`:**
````markdown
# SvelteKit CRUD Pattern (Working Example)

This is the proven pattern that works well across projects.

## File Structure
```
routes/
  items/
    +page.svelte          (list view)
    +page.server.ts       (load + actions)
    [id]/
      +page.svelte        (detail view)
      +page.server.ts     (load + actions)
```

## Complete Implementation

### List Page (+page.server.ts)
```typescript
import { db } from '$lib/server/db';
import { items } from '$lib/server/schema';
import { eq } from 'drizzle-orm';
import { fail } from '@sveltejs/kit';

export const load = async ({ locals }) => {
  const userItems = await db.query.items.findMany({
    where: eq(items.userId, locals.user.id),
    orderBy: (items, { desc }) => [desc(items.createdAt)]
  });
  
  return { items: userItems };
};

export const actions = {
  create: async ({ request, locals }) => {
    const formData = await request.formData();
    const title = formData.get('title');
    
    if (!title || typeof title !== 'string') {
      return fail(400, { error: 'Title required' });
    }
    
    await db.insert(items).values({
      title,
      userId: locals.user.id,
      createdAt: new Date()
    });
    
    return { success: true };
  },
  
  delete: async ({ request, locals }) => {
    const formData = await request.formData();
    const id = formData.get('id');
    
    await db.delete(items).where(
      and(
        eq(items.id, id),
        eq(items.userId, locals.user.id) // Security check
      )
    );
    
    return { success: true };
  }
};
```

### List Page (+page.svelte)
```html
<script lang="ts">
  import { enhance } from '$app/forms';
  export let data;
  export let form;
</script>

<form method="POST" action="?/create" use:enhance>
  <input 
    name="title" 
    placeholder="New item..." 
    required 
  />
  <button type="submit">Add</button>
</form>

{#if form?.error}
  <p class="error">{form.error}</p>
{/if}

<ul>
  {#each data.items as item}
    <li>
      <a href="/items/{item.id}">{item.title}</a>
      <form method="POST" action="?/delete" use:enhance>
        <input type="hidden" name="id" value={item.id} />
        <button type="submit">Delete</button>
      </form>
    </li>
  {/each}
</ul>
```

## Key Patterns to Notice

1. **Load function returns data**: Available as `data` prop
2. **Actions handle mutations**: Create, update, delete
3. **use:enhance**: Progressive enhancement (works without JS)
4. **Security**: Always filter by userId
5. **Timestamps**: Added automatically
6. **No API layer**: Direct DB access in server files

## Extend This Pattern

When building similar features, follow this exact structure and adapt:
- Change `items` to your entity
- Add validation schema (Zod)
- Add more fields to form
- Keep the security pattern (always check userId/orgId)
````

