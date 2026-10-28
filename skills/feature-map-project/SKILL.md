---
name: feature-map-project
description: Use when the user refers to a product feature, page, route, modal, or component in their own shorthand or UI labels, when starting work in a repository, or when a file path is found or moved during a task. Maps feature shorthand to exact codebase paths in FEATURE_MAP.md.
---

# Feature map for projects

Keep a `FEATURE_MAP.md` under 100 lines at the repository root or `.agents/FEATURE_MAP.md` that maps the user's everyday words for pages, modals, flows, and API routes to exact file paths.

## Why this exists

Developers name features by their product terms and UI labels, such as "the checkout drawer", "the pricing toggle", "the Stripe webhook", or localized button copy. Codebases group files by technical directories such as `src/features/billing/components/PlanSelector.tsx`. Without a map, coding agents run repeated `rg` and `glob` searches at the start of every task to find the same files.

## When to use

- The user names a feature, page, modal, component, or API route using product shorthand or UI text.
- You are starting a task in a repository and need the relevant files.
- You just built a new feature, moved or renamed a mapped file, or ran several searches to find an entrypoint.

## Workflow

### 1. Read the map before searching

Check whether `FEATURE_MAP.md` or `.agents/FEATURE_MAP.md` exists before running repo-wide `rg`, `grep`, or `find` searches for a feature name. If it exists, read it first and open the listed file paths, routes, and state stores directly.

### 2. Create the map if missing

If no `FEATURE_MAP.md` exists when this skill runs:
1. Scan the repository's top-level routes and directories such as `app/`, `src/pages/`, `src/components/`, `src/features/`, and `server/`.
2. Create `FEATURE_MAP.md` at the repo root from `templates/FEATURE_MAP.template.md`.
3. Fill in the main user-facing routes, modals, state stores, and API handlers in under 100 lines.

### 3. Act when the path exists

When `FEATURE_MAP.md` points to a file path that exists on disk, open it and start working. Do not ask the user to confirm a mapped path that exists and matches the task. Ask only if the mapped file was deleted or moved with no clear replacement, or if the user's request could mean two different features.

### 4. Update the map when files change

If you move, split, or rename a file during a task, update its row in `FEATURE_MAP.md` before finishing. When you build a new feature or spend multiple searches finding a non-obvious file, add a row mapping the user's phrasing to the file path.

## Rules for `FEATURE_MAP.md`

- Keep the file under 100 lines. Each row maps the user's words to an exact file path and a short responsibility note.
- Record the exact words the user types, including localized UI labels, button text, or team nicknames alongside the English feature name.
- Read the target file to check its current code before editing.
