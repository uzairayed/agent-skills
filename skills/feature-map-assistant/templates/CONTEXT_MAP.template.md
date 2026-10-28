# Personal Assistant Context & Feature Map

> Fast lookup from the user's everyday shorthand to exact machine IDs, local filesystem paths, cloud document IDs, and project feature maps. Keep under ~100 lines.

## 1. Registered Machines & Devices
| User Shorthand | Machine ID / Target | OS & Notes |
| :--- | :--- | :--- |
| "my desktop", "my PC", "Windows PC" | `<machine-id-desktop>` | Windows 11 workstation (primary dev machine) |
| "my Mac", "my laptop", "MacBook" | `<machine-id-macbook>` | macOS laptop |
| "shared box", "sandbox", "agent computer" | *(omit `machineId`)* | Persistent Linux `/workspace` shared across agents |

## 2. Local Repositories & Directories
| User Shorthand | Machine | Exact Path | Notes |
| :--- | :--- | :--- | :--- |
| "acme web", "the storefront repo" | Desktop PC | `D:\projects\acme-web` | Next.js storefront (see repo `FEATURE_MAP.md` below) |
| "acme web" (on Mac) | MacBook | `/Users/alex/projects/acme-web` | Local Mac checkout of `acme-web` |
| "automation scripts", "scraper folder" | Desktop PC | `E:\tools\automation-scripts` | Python utilities and local cron jobs |

## 3. Cloud Workspaces (Notion, Drive, Dashboards)
| User Shorthand | Service | Exact ID / URL | Notes |
| :--- | :--- | :--- | :--- |
| "product roadmap", "sprint board" | Notion | `<notion-database-id>` | Main engineering tasks database |
| "meeting notes", "weekly sync doc" | Notion | `<notion-page-id>` | Recurring team sync notes page |
| "brand assets", "design folder" | Google Drive | `<drive-folder-id>` | Logos, ad creatives, and export templates |
| "analytics", "session replays" | PostHog / Clarity | Project `<project-id>` | Production web analytics and heatmaps |

## 4. Chats, Channels & Specialist Agents
| User Shorthand | Platform / Agent | Identifier / Handle | Notes |
| :--- | :--- | :--- | :--- |
| "engineering channel", "dev chat" | Slack | `#eng-platform` (`<channel-id>`) | Core engineering alerts and releases |
| "Planner", "spec planner" | Subagent | `@Planner` | Writes TDD implementation plans in `/workspace/plans/` |
| "Dev agent", "frontend builder" | Subagent | `@Dev` | Executes implementation plans and opens PRs |

## 5. Active Project Quick-Map (`acme-web`)
| Feature Shorthand / UI Copy | Relative Path in Repo | Responsibility |
| :--- | :--- | :--- |
| "express buy page", "ad landing" | `src/app/buy/page.tsx` | Single-screen mobile landing route for ad traffic |
| "customizer wizard", "generator" | `src/app/generator/page.tsx` | 4-step product customization flow |
| "checkout modal", "Buy Now" | `src/components/checkout/OrderModal.tsx` | Address lookup, validation, and payment session init |
| "community templates", "made by others" | `src/components/generator/CommunityTemplates.tsx` | Searchable gallery of customer-created templates |
