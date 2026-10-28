---
name: feature-map-assistant
description: Use whenever the user refers to a device, machine, chat channel, Notion page/database, Google Drive folder, analytics project, local repo path, or specialist subagent in everyday shorthand. Resolves personal and workspace shorthand via a persistent FEATURE_MAP.md so the assistant acts immediately without re-asking for IDs, URLs, or paths.
---

# Feature & Context Map (Personal Assistant / Grok Bot Edition)

Maintain a concise, high-signal `FEATURE_MAP.md` (under ~100 lines) in your persistent agent store and mirrored at `/workspace/FEATURE_MAP.md` that maps the user's everyday shorthand for devices, chat channels, cloud docs, local project paths, and specialist subagents to their exact IDs, paths, and URLs.

## Why This Exists
In multi-device personal assistant environments (such as Grok Bot), users speak naturally: *"check the team chat"*, *"update the roadmap in Notion"*, *"run the build on my desktop"*, or *"open the client repo on my Mac"*. Without a persistent context map, the assistant wastes turns searching across MCP connectors or repeatedly asking the user for Notion database IDs, machine IDs, folder URLs, or local disk paths they already provided in earlier sessions.

## When to Use
- The user refers to a computer, phone, chat room, Notion page/database, Drive folder, analytics project, local directory, or subagent in shorthand (`"on my PC"`, `"the weekly tracker"`, `"the design folder"`, `"the main web repo"`).
- You are starting a task on a known project or workspace resource without explicit URLs or IDs in the prompt.
- You are delegating work to specialist subagents (Planner, Dev, QA) that need immediate access to workspace paths and repo locations.
- A new recurring resource, machine, Notion database, chat channel, or local directory is established during a conversation.

## Workflow

### 1. Consult First (Before Searching or Asking)
- Before searching across MCP connectors (Notion, Drive, Slack) or asking the user *"which path / which database?"*, read `/workspace/FEATURE_MAP.md` (or your persistent store's `FEATURE_MAP.md`).
- If the task touches a mapped local repository that also has its own `<repo-root>/FEATURE_MAP.md`, read that project map too before searching the codebase.

### 2. Cold-Start Bootstrap
- If no `FEATURE_MAP.md` exists yet in `/workspace/FEATURE_MAP.md` or your persistent store:
  1. Initialize it from `templates/CONTEXT_MAP.template.md`.
  2. Populate any already-known machine IDs (e.g., from `ListMachines`), active repo paths, or connected workspace resources.
  3. Never hardcode private IDs inside `SKILL.md` itself—keep all user-specific IDs inside `FEATURE_MAP.md`.

### 3. Act Without Re-Asking
- When `FEATURE_MAP.md` maps the user's phrase to a concrete machine ID, Notion database ID, Drive folder ID, or local path that exists and is reachable, **act on it immediately**.
- Do **not** interrupt the user to re-confirm a known path, machine, or document ID that works.
- Only ask the user to confirm when:
  - A mapped path or resource no longer exists, was moved, or is offline.
  - The user's shorthand genuinely matches two different mapped targets and context does not disambiguate them.
  - The action itself is irreversible or externally visible (e.g., sending an external message, merging to production, deleting data).

### 4. Keep It Current (Progressive Capture)
- Whenever the user shares a new recurring local directory, repository path, Notion database, Drive folder, analytics dashboard, or chat channel, append or update its 1-line entry in `FEATURE_MAP.md`.
- If a directory moves or a machine ID changes, heal the entry in `FEATURE_MAP.md` immediately.

### 5. Shared Multi-Agent Visibility
- Keep `/workspace/FEATURE_MAP.md` synced with your persistent store copy so specialist subagents (such as `@Planner`, `@Dev`, or background executors) can read `/workspace/FEATURE_MAP.md` on their first turn and jump straight to the right files and resources.

## Rules for `FEATURE_MAP.md`
- **Keep it scannable and under ~100 lines:** Group by category (`Machines`, `Repositories & Local Paths`, `Notion & Docs`, `Chats & Channels`, `Analytics & Dashboards`, `Subagents`).
- **Record the user's exact words:** Map the exact phrases the user says (`"my desktop"`, `"my laptop"`, `"the sprint board"`, `"acme repo"`) to the exact identifier + a 1-line note.
