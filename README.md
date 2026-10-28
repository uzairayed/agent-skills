# Agent skills

Reusable skills for Cursor, Grok Bot, Claude Code, and Codex using the `skills/<skill-name>/SKILL.md` format.

## Available skills

| Skill | Environment | Description |
| :--- | :--- | :--- |
| [`feature-map-project`](./skills/feature-map-project/SKILL.md) | Cursor, Claude Code, Codex, Windsurf | Maps product terms, UI labels, modals, state stores, and webhooks to exact file paths in `<repo-root>/FEATURE_MAP.md` so agents skip repeated `rg` and `glob` searches. |
| [`feature-map-assistant`](./skills/feature-map-assistant/SKILL.md) | Grok Bot and multi-device assistants | Maps devices, local repo paths, Notion databases, Drive folders, chat channels, and subagents in `/workspace/FEATURE_MAP.md` so your assistant acts without re-asking for IDs or paths. |

## Install

### Using `npx skills`

```bash
# Install one skill by name
npx skills add uzairayed/agent-skills --skill feature-map-project
npx skills add uzairayed/agent-skills --skill feature-map-assistant

# Install every skill in this repo
npx skills add uzairayed/agent-skills
```

### Cursor

Copy a skill folder into `.cursor/skills/` or `.agents/skills/` in your project:

```bash
SKILL_NAME="feature-map-project"
mkdir -p .cursor/skills
git clone --depth 1 https://github.com/uzairayed/agent-skills.git /tmp/uzairayed-agent-skills
cp -R "/tmp/uzairayed-agent-skills/skills/${SKILL_NAME}" ".cursor/skills/${SKILL_NAME}"
rm -rf /tmp/uzairayed-agent-skills
```

### Claude Code

```bash
SKILL_NAME="feature-map-project"
mkdir -p .claude/skills
git clone --depth 1 https://github.com/uzairayed/agent-skills.git /tmp/uzairayed-agent-skills
cp -R "/tmp/uzairayed-agent-skills/skills/${SKILL_NAME}" ".claude/skills/${SKILL_NAME}"
rm -rf /tmp/uzairayed-agent-skills
```

### Grok Bot

```bash
SKILL_NAME="feature-map-assistant"
mkdir -p /home/box/agent-data/workflows
git clone --depth 1 https://github.com/uzairayed/agent-skills.git /tmp/uzairayed-agent-skills
cp -R "/tmp/uzairayed-agent-skills/skills/${SKILL_NAME}" "/home/box/agent-data/workflows/${SKILL_NAME}"
rm -rf /tmp/uzairayed-agent-skills
```

## Skill details

### `feature-map-project`

Read [`skills/feature-map-project/SKILL.md`](./skills/feature-map-project/SKILL.md) and [`skills/feature-map-project/templates/FEATURE_MAP.template.md`](./skills/feature-map-project/templates/FEATURE_MAP.template.md).

Developers refer to features by their product names and UI labels, such as "fix validation in the checkout drawer" or "make the Buy Now button focus the name input". Codebases group files by technical directories like `src/features/checkout/CheckoutDrawer.tsx`. Without a map, coding agents run 5 to 15 `rg` and `glob` searches at the start of each task to find the same files.

With this skill installed:
1. The agent reads `<repo-root>/FEATURE_MAP.md` or `.agents/FEATURE_MAP.md` before running repo searches.
2. If no `FEATURE_MAP.md` exists, the agent scans `app/`, `src/`, `pages/`, and `components/` to create a map under 100 lines from [`FEATURE_MAP.template.md`](./skills/feature-map-project/templates/FEATURE_MAP.template.md).
3. When a mapped file exists on disk, the agent opens it directly without asking you to confirm the path.
4. When a task moves a file or adds a feature, the agent updates `FEATURE_MAP.md` before finishing.

<details>
<summary>Example project <code>FEATURE_MAP.md</code></summary>

```markdown
# Feature map

## Pages and routes
| User shorthand or UI label | Route | Files and notes |
| :--- | :--- | :--- |
| "home", "landing page" | `/` | `src/app/page.tsx`, `src/components/marketing/Hero.tsx` |
| "express buy page", "ad landing" | `/buy` | `src/app/buy/page.tsx`, single-screen mobile ad flow |
| "customizer", "generator" | `/generator` | `src/app/generator/page.tsx`, 4-step customization wizard |

## Modals and components
| User shorthand or UI label | Component path | Responsibility |
| :--- | :--- | :--- |
| "checkout drawer", "Buy Now" | `src/features/checkout/CheckoutDrawer.tsx` | Slide-over cart, bundle tiers, and order summary |
| "payment modal", "pay step" | `src/features/checkout/PaymentModal.tsx` | Phone and email validation and payment initiation |
| "template picker", "browse templates" | `src/features/templates/TemplateGallerySheet.tsx` | Searchable bottom sheet of community designs |

## State and APIs
| Shorthand | File path | Responsibility |
| :--- | :--- | :--- |
| "editor state", "canvas store" | `src/stores/useEditorStore.ts` | Zustand store for active design state |
| "preview generator API" | `src/app/api/render-preview/route.ts` | Server-side PNG renderer and storage upload |
```
</details>

### `feature-map-assistant`

Read [`skills/feature-map-assistant/SKILL.md`](./skills/feature-map-assistant/SKILL.md) and [`skills/feature-map-assistant/templates/FEATURE_MAP.template.md`](./skills/feature-map-assistant/templates/FEATURE_MAP.template.md).

When talking to a multi-device assistant like Grok Bot, you refer to machines, chats, cloud docs, and local repos by short names such as "run the build on my PC", "update the sprint board in Notion", or "check the storefront repo on my Mac". Without a saved map, the assistant searches across connectors or asks for IDs and paths you have already given it.

With this skill installed:
1. The assistant reads `/workspace/FEATURE_MAP.md` before searching connectors or asking for URLs and IDs.
2. On first use, the assistant creates `/workspace/FEATURE_MAP.md` from [`FEATURE_MAP.template.md`](./skills/feature-map-assistant/templates/FEATURE_MAP.template.md). Private IDs stay in `FEATURE_MAP.md` and out of `SKILL.md`.
3. When a mapped machine ID, Notion database, Drive folder, or local path is reachable, the assistant uses it right away and only asks if a target moved, went offline, or matches multiple entries.
4. Keeping the map at `/workspace/FEATURE_MAP.md` lets subagents read the same paths on their first turn.

<details>
<summary>Example assistant <code>FEATURE_MAP.md</code></summary>

```markdown
# Assistant context and feature map

## 1. Machines and devices
| User shorthand | Machine ID | OS and notes |
| :--- | :--- | :--- |
| "my desktop", "my PC" | `<machine-id-desktop>` | Windows 11 workstation |
| "my Mac", "my laptop" | `<machine-id-macbook>` | macOS laptop |
| "shared box", "sandbox" | omit `machineId` | Shared Linux `/workspace` |

## 2. Local repositories and directories
| User shorthand | Machine | Exact path | Notes |
| :--- | :--- | :--- | :--- |
| "acme web", "storefront repo" | Desktop PC | `D:\projects\acme-web` | Next.js storefront |
| "automation scripts" | Desktop PC | `E:\tools\automation-scripts` | Python utilities and cron jobs |

## 3. Cloud workspaces and subagents
| User shorthand | Service or role | Exact ID or handle | Notes |
| :--- | :--- | :--- | :--- |
| "sprint board", "roadmap" | Notion | `<notion-database-id>` | Main engineering tasks database |
| "design folder" | Google Drive | `<drive-folder-id>` | Brand assets and ad creatives |
| "Planner" | Subagent | `@Planner` | Writes TDD plans in `/workspace/plans/` |
```
</details>

## Adding a skill

Each skill lives in `skills/<skill-name>/` with a `SKILL.md` file and optional templates or scripts:

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
    └── <skill-name>/
        └── SKILL.md
```

To add a skill:
1. Create `skills/<skill-name>/SKILL.md` with `name` and `description` in the YAML frontmatter.
2. Put any templates or helper scripts inside `skills/<skill-name>/`.
3. Add a row to the table at the top of `README.md`.

## License

MIT, Uzair Ahmed
