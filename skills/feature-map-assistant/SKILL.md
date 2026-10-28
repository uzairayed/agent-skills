---
name: feature-map-assistant
description: Use when the user refers to a device, machine, chat channel, Notion page or database, Google Drive folder, analytics project, local repo path, or subagent in everyday shorthand. Resolves workspace shorthand through a persistent FEATURE_MAP.md so the assistant acts without re-asking for IDs, URLs, or paths.
---

# Feature and context map for assistants

Keep a `FEATURE_MAP.md` under 100 lines in your persistent agent store and mirrored at `/workspace/FEATURE_MAP.md` that maps the user's shorthand for devices, chat channels, cloud docs, local project paths, and subagents to their exact IDs, paths, and URLs.

## Why this exists

In multi-device assistants like Grok Bot, users say things like "check the team chat", "update the roadmap in Notion", "run the build on my desktop", or "open the client repo on my Mac". Without a saved map, the assistant searches across MCP connectors or asks the user for Notion database IDs, machine IDs, folder URLs, or local disk paths they already gave in earlier chats.

## When to use

- The user refers to a computer, phone, chat room, Notion page or database, Drive folder, analytics project, local directory, or subagent by a short name.
- You are starting a task on a known project or workspace resource without URLs or IDs in the message.
- You are handing work to subagents such as Planner, Dev, or QA that need workspace paths and repo locations.
- The user introduces a new recurring machine, Notion database, chat channel, or local directory during a conversation.

## Workflow

### 1. Read the map before searching or asking

Read `/workspace/FEATURE_MAP.md` or your persistent store's `FEATURE_MAP.md` before searching across MCP connectors or asking the user which path or database they mean. If the task touches a local repository that also has its own `<repo-root>/FEATURE_MAP.md`, read that project map too before searching the codebase.

### 2. Create the map if missing

If no `FEATURE_MAP.md` exists in `/workspace/FEATURE_MAP.md` or your persistent store:
1. Copy `templates/FEATURE_MAP.template.md` to `/workspace/FEATURE_MAP.md`.
2. Fill in known machine IDs from `ListMachines`, active repo paths, and connected workspace resources.
3. Keep all private IDs inside `FEATURE_MAP.md` and never hardcode them inside `SKILL.md`.

### 3. Act when the target is reachable

When `FEATURE_MAP.md` maps the user's words to a machine ID, Notion database ID, Drive folder ID, or local path that exists and responds, use it right away. Do not ask the user to confirm a known path, machine, or document ID that works. Ask only when:
- A mapped path or resource no longer exists, moved, or is offline.
- The user's shorthand matches two different mapped entries and the conversation does not settle which one they mean.
- The action itself sends an external message, merges to production, or deletes data.

### 4. Update the map as new resources appear

When the user shares a new recurring local directory, repository path, Notion database, Drive folder, analytics dashboard, or chat channel, add or update its row in `FEATURE_MAP.md`. If a directory moves or a machine ID changes, update the row right away.

### 5. Mirror to `/workspace/FEATURE_MAP.md`

Keep `/workspace/FEATURE_MAP.md` synced with your persistent store copy so subagents can read `/workspace/FEATURE_MAP.md` on their first turn and find the right files and resources.

## Rules for `FEATURE_MAP.md`

- Keep it under 100 lines, grouped into sections for machines, local repositories, cloud docs, chat channels, analytics, and subagents.
- Map the exact phrases the user says, such as "my desktop", "my laptop", "the sprint board", or "acme repo", to the exact identifier and a short note.
