# Repository Guidelines

## Project Structure & Module Organization
Content is staged by tier so agents load the right context. `01_base/` stores global preferences, `02_domain/` houses discipline playbooks (e.g., `js_and_ts_rules.md`), and `03_stack/` documents technology rules. `04_project/` must hold only the active brief, `05_examples/` keeps reusable patterns, and `06_memory/` preserves long-lived learnings. 

Read the intro file in each directory before adding or renaming assets to stay aligned.

## Coding Style & Naming Conventions
Write tight Markdown with descriptive headings and action bullets; mirror the tone already set in the templates. Follow the directory intros for format (title case headings, fenced code blocks with language hints). For code samples, follow `02_domain/js_and_ts_rules.md`: TypeScript examples, early returns, object-in/object-out signatures, and named exports. Keep filenames kebab-case to match the corpus.

## Agent-Specific Notes
Load the layer order (base → domain → stack → project) before editing so you understand downstream effects. When sunsetting material, move it to `05_examples/` instead of deleting to preserve reusable patterns.
