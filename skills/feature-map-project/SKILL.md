---
name: feature-map-project
description: Use whenever the user refers to a product feature, UI surface, route, modal, or domain concept in their own everyday shorthand (including non-English UI labels), when starting work in a repository, or when a file path is discovered or moved during a task. Maps human feature shorthand directly to exact codebase paths in a concise FEATURE_MAP.md.
---

# Feature Map (Project Edition)

Maintain a concise, high-signal `FEATURE_MAP.md` (under ~100 lines) at the repository root (or `.agents/FEATURE_MAP.md`) that maps the developer's everyday shorthand for product surfaces, modals, flows, and API handlers directly to exact file paths.

## Why This Exists
Developers speak in product and UI shorthand (`"the checkout drawer"`, `"the pricing toggle"`, `"the Stripe webhook"`, or localized UI copy like `"Comprar ahora button"`), while codebases organize files by technical layers (`src/features/billing/components/PlanSelector.tsx`). Without a feature map, coding agents burn thousands of tokens and extra tool calls running broad `rg` and `glob` searches at the start of every session.

## When to Use
- The user references a feature, page, modal, component, API route, or domain concept using everyday shorthand or localized UI text.
- You are starting a task in a repository and need to locate the relevant entrypoints quickly.
- You just finished building a new feature area, renamed/moved a mapped file during refactoring, or spent multiple searches tracking down a non-obvious entrypoint.

## Workflow

### 1. Consult First (Before Broad Search)
- Before running repo-wide `rg`, `grep`, or `find` searches for a feature name, check whether `FEATURE_MAP.md` (or `.agents/FEATURE_MAP.md`) exists.
- If it exists, read it first and resolve the user's shorthand directly to the listed file paths, routes, and state stores.

### 2. Cold-Start Bootstrap
- If no `FEATURE_MAP.md` exists and the user invokes this skill (or asks to map the repo):
  1. Scan the repository's top-level routes and entrypoints (`app/`, `src/pages/`, `src/components/`, `src/features/`, `server/`, `prisma/schema.prisma`, etc.).
  2. Create `FEATURE_MAP.md` at the repo root using `templates/FEATURE_MAP.template.md` as the starting structure.
  3. Populate it with the core user-facing routes, high-traffic UI components, state stores, and API handlers in under 100 lines.

### 3. Act Immediately When Verified
- When `FEATURE_MAP.md` points to a concrete file path that exists on disk, open and work on it directly.
- Do **not** interrupt the user to re-confirm a mapped file path that exists and matches the task.
- Only ask for clarification when a mapped file was deleted/moved and no clear replacement exists, or when the user's request is genuinely ambiguous between two distinct features.

### 4. Self-Healing & Progressive Capture
- **Moved or renamed files:** If you move, split, or rename a file during a task, update its corresponding line in `FEATURE_MAP.md` before finishing.
- **New features or hard-to-find entrypoints:** When you build a new feature surface or spend multiple searches locating a non-obvious component, add a 1-line entry mapping the user's natural phrasing to the exact path.

## Rules for `FEATURE_MAP.md`
- **Keep it under ~100 lines:** High signal, zero prose fluff. Every entry is `"user shorthand" -> exact/file/path.tsx — 1-line responsibility note`.
- **Use the user's exact vocabulary:** Include the words the user actually types, including localized UI labels, button text, or internal team nicknames alongside the English feature name.
- **Verify live code:** The map gets you to the right file in one hop; always read the target file to verify its current implementation before editing.
