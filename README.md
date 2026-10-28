# Agent Skills by [@uzairayed](https://github.com/uzairayed)

A curated collection of reusable agent skills for **Cursor**, **Grok Bot**, **Claude Code**, **Codex**, and any agent supporting the standard `skills/<skill-name>/SKILL.md` specification.

---

## Available Skills

| Skill | Target Environment | Description |
| :--- | :--- | :--- |
| [**`feature-map-project`**](./skills/feature-map-project/SKILL.md) | **Project & Repo Work** (Cursor, Claude Code, Codex, Windsurf) | Maps everyday product shorthand, UI labels, modals, state stores, and webhooks directly to exact repo file paths in `<repo-root>/FEATURE_MAP.md` so agents skip repetitive `rg` / `glob` discovery searches. |
| [**`feature-map-assistant`**](./skills/feature-map-assistant/SKILL.md) | **Personal Assistants & Multi-Device Agents** (Grok Bot, persistent workspace agents) | Maps devices (`"my PC"`, `"my Mac"`), local repo folders, Notion databases, Drive folders, chat channels, and specialist subagents in `/workspace/FEATURE_MAP.md` so your assistant acts immediately without re-asking for IDs or paths. |

---

## Quick Install

### 1. Using the `skills` CLI (`npx skills`)

Install a specific skill (recommended) or install all skills from this catalog:

```bash
# Install a single skill by name
npx skills add uzairayed/agent-skills --skill feature-map-project
npx skills add uzairayed/agent-skills --skill feature-map-assistant

# Or install every skill in this repository
npx skills add uzairayed/agent-skills
```

### 2. Cursor IDE (`.cursor/skills/<skill-name>/`)

Copy any skill from `skills/<skill-name>` into your project's `.cursor/skills/` (or `.agents/skills/`) directory:

```bash
SKILL_NAME="feature-map-project"
mkdir -p .cursor/skills
git clone --depth 1 https://github.com/uzairayed/agent-skills.git /tmp/uzairayed-agent-skills
cp -R "/tmp/uzairayed-agent-skills/skills/${SKILL_NAME}" ".cursor/skills/${SKILL_NAME}"
rm -rf /tmp/uzairayed-agent-skills
```

### 3. Claude Code (`.claude/skills/<skill-name>/`)

```bash
SKILL_NAME="feature-map-project"
mkdir -p .claude/skills
git clone --depth 1 https://github.com/uzairayed/agent-skills.git /tmp/uzairayed-agent-skills
cp -R "/tmp/uzairayed-agent-skills/skills/${SKILL_NAME}" ".claude/skills/${SKILL_NAME}"
rm -rf /tmp/uzairayed-agent-skills
```

### 4. Grok Bot (`/home/box/agent-data/workflows/<skill-name>/`)

Install any skill into your Grok Bot workflows directory:

```bash
SKILL_NAME="feature-map-assistant"
mkdir -p /home/box/agent-data/workflows
git clone --depth 1 https://github.com/uzairayed/agent-skills.git /tmp/uzairayed-agent-skills
cp -R "/tmp/uzairayed-agent-skills/skills/${SKILL_NAME}" "/home/box/agent-data/workflows/${SKILL_NAME}"
rm -rf /tmp/uzairayed-agent-skills
```

---

## Skill Details

### 1. `feature-map-project` — Fast Codebase Feature Resolution

- **Skill file:** [`skills/feature-map-project/SKILL.md`](./skills/feature-map-project/SKILL.md)
- **Template:** [`skills/feature-map-project/templates/FEATURE_MAP.template.md`](./skills/feature-map-project/templates/FEATURE_MAP.template.md)

**The problem it solves:** Developers talk in product and UI shorthand (*"fix validation in the checkout drawer"*, *"make the 'Buy Now' button focus the name input"*), while codebases organize files by technical layers (`src/features/checkout/CheckoutDrawer.tsx`). Without a map, coding agents burn 5–15 tool calls running `rg` and `glob` searches at the start of every task.

**How it works:**
1. **Consult first:** Reads `<repo-root>/FEATURE_MAP.md` (or `.agents/FEATURE_MAP.md`) before running broad repo searches.
2. **Cold-start bootstrap:** If no `FEATURE_MAP.md` exists yet, scans `app/`, `src/`, `pages/`, and `components/` to generate a `<100 line` map from [`FEATURE_MAP.template.md`](./skills/feature-map-project/templates/FEATURE_MAP.template.md).
3. **Act immediately when verified:** Opens mapped files directly without asking the user to re-confirm existing paths.
4. **Self-healing:** Updates the 1-line entry whenever files move during refactoring or new features are created.

<details>
<summary><strong>Example Project <code>FEATURE_MAP.md</code></strong></summary>

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
</details>

---

### 2. `feature-map-assistant` — Multi-Device & Workspace Context Map

- **Skill file:** [`skills/feature-map-assistant/SKILL.md`](./skills/feature-map-assistant/SKILL.md)
- **Template:** [`skills/feature-map-assistant/templates/FEATURE_MAP.template.md`](./skills/feature-map-assistant/templates/FEATURE_MAP.template.md)

**The problem it solves:** In personal assistant environments (like Grok Bot), users refer to machines, chats, cloud docs, and local repos in natural shorthand (*"run the build on my PC"*, *"update the sprint board in Notion"*, *"check the storefront repo on my Mac"*). Without a persistent context map, the assistant wastes turns searching across MCP connectors or re-asking for IDs and paths.

**How it works:**
1. **Consult first:** Reads `/workspace/FEATURE_MAP.md` (or the agent's persistent store `FEATURE_MAP.md`) before searching across connectors or asking for URLs/IDs.
2. **Cold-start bootstrap:** Initializes `/workspace/FEATURE_MAP.md` from [`FEATURE_MAP.template.md`](./skills/feature-map-assistant/templates/FEATURE_MAP.template.md) on first run—never hardcoding private IDs in `SKILL.md`.
3. **Act without re-asking:** Resolves known machine IDs, Notion databases, Drive folders, and local paths immediately; only asks when a target moved, is offline, or is ambiguous.
4. **Shared multi-agent visibility:** Mirrors the map to `/workspace/FEATURE_MAP.md` so specialist subagents (`@Planner`, `@Dev`, `@QA`) resolve paths on their first turn.

<details>
<summary><strong>Example Personal Assistant <code>FEATURE_MAP.md</code></strong></summary>

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
</details>

---

## Repository Structure & Adding New Skills

Every skill lives in its own folder under `skills/<skill-name>/` with a `SKILL.md` containing YAML frontmatter (`name` and `description`) plus optional helper scripts or `templates/`:

```text
agent-skills/
├── README.md
├── LICENSE
└── skills/
    ├── feature-map-project/
    │   ├── SKILL.md
    │   └── templates/
    │       └── FEATURE_MAP.template.md
    ├── feature-map-assistant/
    │   ├── SKILL.md
    │   └── templates/
    │       └── FEATURE_MAP.template.md
    └── <your-new-skill>/
        ├── SKILL.md
        └── templates/ (optional)
```

To add a new skill to this repository:
1. Create `skills/<skill-name>/SKILL.md` with `name: <skill-name>` and a clear trigger `description` in the frontmatter.
2. Add any supporting templates or scripts inside `skills/<skill-name>/`.
3. Add a row to the **Available Skills** table in `README.md`.

## License

MIT © Uzair Ahmed
