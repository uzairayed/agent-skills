# Feature Map (`<project-name>`)

> Fast lookup from everyday feature shorthand and UI labels to exact codebase files. Keep under ~100 lines; update paths whenever files move.

## Core Pages & Routes
| User Shorthand / UI Label | Route / Entrypoint | Key Files & Notes |
| :--- | :--- | :--- |
| "home", "landing page", "hero" | `/` | `src/app/page.tsx`, `src/components/marketing/Hero.tsx` |
| "dashboard", "overview" | `/dashboard` | `src/app/(app)/dashboard/page.tsx` |
| "Editor", "canvas page" | `/editor/[id]` | `src/app/(app)/editor/[id]/page.tsx` — main interactive workspace |
| "settings", "billing page" | `/settings/billing` | `src/app/(app)/settings/billing/page.tsx` |

## UI Surfaces, Modals & Flows
| User Shorthand / UI Label | Component Path | Responsibility |
| :--- | :--- | :--- |
| "checkout drawer", "cart", "Buy Now button" | `src/features/checkout/CheckoutDrawer.tsx` | Slide-over cart, quantity selector, and order summary |
| "payment modal", "pay step" | `src/features/checkout/PaymentModal.tsx` | Payment method selection, phone/email validation, and submit |
| "template picker", "browse templates" | `src/features/templates/TemplateGallerySheet.tsx` | Searchable bottom sheet / modal of community templates |
| "onboarding wizard", "signup steps" | `src/features/onboarding/OnboardingWizard.tsx` | 4-step account setup wizard and step gating |

## State Stores & Hooks
| Shorthand / Concept | File Path | Responsibility |
| :--- | :--- | :--- |
| "editor state", "canvas store" | `src/stores/useEditorStore.ts` | Zustand store holding active design state and undo history |
| "cart state", "pricing logic" | `src/hooks/useCartTotals.ts` | Bundle discounts, shipping tiers, and line-item calculation |

## API Routes, Webhooks & Background Jobs
| Shorthand / Concept | File Path | Responsibility |
| :--- | :--- | :--- |
| "Stripe webhook", "payment webhook" | `src/app/api/webhooks/stripe/route.ts` | Handles `checkout.session.completed` and order fulfillment |
| "preview generator", "render image API" | `src/app/api/render-preview/route.ts` | Server-side PNG composite renderer + cloud storage upload |
| "abandoned cart sync", "email sync" | `src/lib/integrations/marketing-sync.ts` | Pushes cart abandonment events and preview URLs to ESP |

## Commands & Verification
- **Dev server:** `npm run dev`
- **Targeted tests:** `npx vitest run <path-to-test>`
- **Typecheck:** `npx tsc --noEmit`
