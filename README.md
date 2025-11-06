# Context Engineering Template

This repository provides a structured template for context engineering, designed to help you organize and manage the context you provide to large language models (LLMs). By separating context into distinct layers, you can create a more efficient and effective workflow for both new and existing projects. This template is designed to be a starting point, and you are encouraged to adapt it to your specific needs.

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

## Layer Breakdown

Each directory in this repository represents a layer of context. For a detailed explanation of each layer's purpose and an example of how to structure documents within it, please refer to the `01_{layer}_intro.md` file in each directory.

*   **[01_base/01_base_layer_intro.md](01_base/01_base_layer_intro.md):** This layer is for defining personality, communication style, and universal preferences.

*   **[02_domain/01_domain_layer_intro.md](02_domain/01_domain_layer_intro.md):** This layer is for dictating specifics on how to approach programming, problem-solving, and code organization.

*   **[03_stack/01_stack_layer_intro.md](03_stack/01_stack_layer_intro.md):** This layer is for specifics on how to leverage different technologies and stacks.

*   **[04_project/01_project_layer_intro.md](04_project/01_project_layer_intro.md):** This layer is for project-specific context, including architecture, database schemas, and active components.

*   **[05_examples/01_examples_intro.md](05_examples/01_examples_intro.md):** This layer is for crucial boilerplate and working examples that don't fit well in other documents.

*   **[06_memory/01_memory_layer_intro.md](06_memory/01_memory_layer_intro.md):** This layer is for ongoing memory management between sessions, including active session notes and architectural decisions.

The other files in these directories are personal examples and are not part of the core template.