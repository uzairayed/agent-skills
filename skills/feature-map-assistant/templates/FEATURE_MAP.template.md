# Assistant context and feature map

Lookup from the user's shorthand to machine IDs, local paths, cloud document IDs, and project feature maps. Keep under 100 lines.

## 1. Machines and devices
| User shorthand | Machine ID | OS and notes |
| :--- | :--- | :--- |
| "my desktop", "my PC", "Windows PC" | `<machine-id-desktop>` | Windows 11 workstation |
| "my Mac", "my laptop", "MacBook" | `<machine-id-macbook>` | macOS laptop |
| "shared box", "sandbox", "agent computer" | omit `machineId` | Shared Linux `/workspace` |

## 2. Local repositories and directories
| User shorthand | Machine | Exact path | Notes |
| :--- | :--- | :--- | :--- |
| "acme web", "the storefront repo" | Desktop PC | `D:\projects\acme-web` | Next.js storefront |
| "acme web on Mac" | MacBook | `/Users/alex/projects/acme-web` | Local Mac checkout of `acme-web` |
| "automation scripts", "scraper folder" | Desktop PC | `E:\tools\automation-scripts` | Python utilities and local cron jobs |

## 3. Cloud workspaces
| User shorthand | Service | Exact ID or URL | Notes |
| :--- | :--- | :--- | :--- |
| "product roadmap", "sprint board" | Notion | `<notion-database-id>` | Main engineering tasks database |
| "meeting notes", "weekly sync doc" | Notion | `<notion-page-id>` | Recurring team sync notes page |
| "brand assets", "design folder" | Google Drive | `<drive-folder-id>` | Logos, ad creatives, and export templates |
| "analytics", "session replays" | PostHog or Clarity | Project `<project-id>` | Production web analytics and heatmaps |

## 4. Chats, channels, and subagents
| User shorthand | Platform or role | Identifier | Notes |
| :--- | :--- | :--- | :--- |
| "engineering channel", "dev chat" | Slack | `#eng-platform`, `<channel-id>` | Core engineering alerts and releases |
| "Planner", "spec planner" | Subagent | `@Planner` | Writes TDD implementation plans in `/workspace/plans/` |
| "Dev agent", "frontend builder" | Subagent | `@Dev` | Runs implementation plans and opens PRs |

## 5. Active project quick-map (`acme-web`)
| Feature shorthand or UI copy | Relative path in repo | Responsibility |
| :--- | :--- | :--- |
| "express buy page", "ad landing" | `src/app/buy/page.tsx` | Single-screen mobile landing route for ad traffic |
| "customizer wizard", "generator" | `src/app/generator/page.tsx` | 4-step product customization flow |
| "checkout modal", "Buy Now" | `src/components/checkout/OrderModal.tsx` | Address lookup, validation, and payment session setup |
| "community templates", "made by others" | `src/components/generator/CommunityTemplates.tsx` | Searchable gallery of customer-created templates |
