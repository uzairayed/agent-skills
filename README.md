# Feature Map Skill (`feature-map-skill`)

Stop your AI coding agents and personal assistants from burning thousands of tokens on repetitive `rg` / `glob` searches or asking *"which folder / which Notion database / which component is that?"* every session.

**`feature-map-skill`** gives your agent a lightweight, self-healing `<100 line` lookup table (`FEATURE_MAP.md`) that maps your **everyday human shorthand** directly to exact file paths, routes, machine IDs, and workspace resources.

---

## The Problem

Humans talk to agents in **product and workspace shorthand**:
- *"Fix the validation error inside the checkout drawer"*
- *"Make the 'Buy Now' button focus the name input when empty"*
- *"Check the sprint board in Notion and run the test suite on my desktop"*

Codebases and workspaces, however, are organized by technical identifiers (`src/features/checkout/components/OrderModal.tsx`, machine UUIDs, Notion database IDs). Without a persistent map:
1. **Coding agents** start every task with 5–15 exploratory `rg` / `find` / `glob` calls just to locate the right component.
2. **Personal assistants** repeatedly ask you to re-confirm local repo paths, machine names, or Notion/Drive links you already gave them yesterday.
3. **Subagents** (`@Planner`, `@Dev`, `@QA`) repeat the exact same discovery searches independently.

---

## Two Skills Included

This repository ships two standalone skills following the standard `skills/<skill-name>/SKILL.md` layout:

| Skill | Best For | What It Maps |
| :--- | :--- | :--- |
| **`feature-map-project`** | **Project-based coding work** (Cursor IDE, Claude Code, Codex, Windsurf) | Maps product features, UI labels (including non-English copy), modals, state stores, and webhooks to exact repo file paths in `<repo-root>/FEATURE_MAP.md`. |
| **`feature-map-assistant`** | **Multi-device personal AI assistants** (Grok Bot, persistent workspace agents) | Maps devices (`"my PC"`, `"my Mac"`), local repo directories, Notion databases, Drive folders, chat channels, and specialist subagents in `/workspace/FEATURE_MAP.md`. |

---

## Quick Install

### 1. Using the `skills` CLI (`npx skills`)

Install both skills (or pick the one that matches your environment):

```bash
# Install all skills from this repo
npx skills add uzairayed/feature-map-skill

# Or install only the project-based coding skill
npx skills add uzairayed/feature-map-skill --skill feature-map-project

# Or install only the personal assistant / Grok Bot skill
npx skills add uzairayed/feature-map-skill --skill feature-map-assistant
```

### 2. Cursor IDE / Claude Code (Project or Global)

Copy `feature-map-project` into your project's `.cursor/skills/` or `.agents/skills/` directory:

```bash
mkdir -p .cursor/skills
git clone https://github.com/uzairayed/feature-map-skill.git /tmp/feature-map-skill
cp -R /tmp/feature-map-skill/skills/feature-map-project .cursor/skills/feature-map-project
rm -rf /tmp/feature-map-skill
```

*(For Claude Code or universal `.agents` setups, copy into `.agents/skills/feature-map-project` instead.)*

### 3. Grok Bot (`/home/box/agent-data/workflows/`)

Install `feature-map-assistant` (and optionally `feature-map-project`) directly into your Grok Bot workflows directory:

```bash
mkdir -p /home/box/agent-data/workflows
git clone https://github.com/uzairayed/feature-map-skill.git /tmp/feature-map-skill
cp -R /tmp/feature-map-skill/skills/feature-map-assistant /home/box/agent-data/workflows/feature-map-assistant
cp -R /tmp/feature-map-skill/skills/feature-map-project /home/box/agent-data/workflows/feature-map-project
rm -rf /tmp/feature-map-skill
```

---

## How the Lifecycle Works

Both skills enforce a 4-step loop:

1. **Consult First:** Before running broad `rg` / `glob` searches or querying MCP connectors, the agent reads `FEATURE_MAP.md` to resolve your shorthand in one hop.
2. **Cold-Start Bootstrap:** If `FEATURE_MAP.md` does not exist yet, the agent uses the bundled template (`templates/FEATURE_MAP.template.md` or `templates/CONTEXT_MAP.template.md`) to generate a clean `<100 line` map of your routes, components, or workspace targets.
3. **Act Without Re-Asking:** When a mapped file path, machine ID, or Notion database exists and is reachable, the agent acts on it immediately without interrupting you to re-confirm.
4. **Self-Healing & Progressive Capture:** When a file is moved during a refactor or a new feature/resource is introduced, the agent updates the 1-line entry automatically.

---

## Example Maps: Project vs. Personal Assistant

### Example 1: `feature-map-project` (`<repo-root>/FEATURE_MAP.md`)

```markdown
# Feature Map (`acme-web`)

## Core Pages & Routes
| User Shorthand / UI Label | Route / Entrypoint | Key Files & Notes |
| :--- | :--- | :--- |
| "home", "landing page" | `/` | `src/app/page.tsx`, `src/components/marketing/Hero.tsx` |
| "express buy page", "ad landing" | `/buy` | `src/app/buy/page.tsx` — single-screen mobile ad flow |
| "customizer", "generator" | `/generator` | `src/app/generator/page.tsx` — 4-step customization wizard |

## UI Surfaces, Modals & Flows
| User Shorthand / UI Label | Component Path | Responsibility |
| :--- | :--- | :--- |
| "checkout drawer", "Buy Now" | `src/features/checkout/CheckoutDrawer.tsx` | Slide-over cart, bundle tiers, and order summary |
| "payment modal", "pay step" | `src/features/checkout/PaymentModal.tsx` | Phone/email validation and payment initiation |
| "template picker", "browse templates" | `src/features/templates/TemplateGallerySheet.tsx` | Searchable bottom sheet of community designs |

## State & APIs
| Shorthand / Concept | File Path | Responsibility |
| :--- | :--- | :--- |
| "editor state", "canvas store" | `src/stores/useEditorStore.ts` | Zustand store for active design state |
| "preview generator API" | `src/app/api/render-preview/route.ts` | Server-side PNG composite renderer + upload |
```

### Example 2: `feature-map-assistant` (`/workspace/FEATURE_MAP.md`)

```markdown
# Personal Assistant Context & Feature Map

## 1. Registered Machines & Devices
| User Shorthand | Machine ID / Target | OS & Notes |
| :--- | :--- | :--- |
| "my desktop", "my PC" | `<machine-id-desktop>` | Windows 11 workstation (primary dev machine) |
| "my Mac", "my laptop" | `<machine-id-macbook>` | macOS laptop |
| "shared box", "sandbox" | *(omit `machineId`)* | Persistent Linux `/workspace` shared across agents |

## 2. Local Repositories & Directories
| User Shorthand | Machine | Exact Path | Notes |
| :--- | :--- | :--- | :--- |
| "acme web", "storefront repo" | Desktop PC | `D:\projects\acme-web` | Next.js storefront |
| "automation scripts" | Desktop PC | `E:\tools\automation-scripts` | Python utilities and cron jobs |

## 3. Cloud Workspaces & Subagents
| User Shorthand | Service / Role | Exact ID / Handle | Notes |
| :--- | :--- | :--- | :--- |
| "sprint board", "roadmap" | Notion | `<notion-database-id>` | Main engineering tasks database |
| "design folder" | Google Drive | `<drive-folder-id>` | Brand assets and ad creatives |
| "Planner" | Subagent | `@Planner` | Writes TDD plans in `/workspace/plans/` |
```

---

## Repository Structure

```text
feature-map-skill/
├── README.md
├── LICENSE
└── skills/
    ├── feature-map-project/
    │   ├── SKILL.md
    │   └── templates/
    │       └── FEATURE_MAP.template.md
    └── feature-map-assistant/
        ├── SKILL.md
        └── templates/
            └── CONTEXT_MAP.template.md
```

## License

MIT © Uzair Ahmed
