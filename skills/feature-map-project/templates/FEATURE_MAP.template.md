# Feature map

Lookup from everyday feature names and UI labels to codebase files. Keep under 100 lines and update paths when files move.

## Pages and routes
| User shorthand or UI label | Route | Files and notes |
| :--- | :--- | :--- |
| "home", "landing page", "hero" | `/` | `src/app/page.tsx`, `src/components/marketing/Hero.tsx` |
| "dashboard", "overview" | `/dashboard` | `src/app/(app)/dashboard/page.tsx` |
| "editor", "canvas page" | `/editor/[id]` | `src/app/(app)/editor/[id]/page.tsx`, main interactive workspace |
| "settings", "billing page" | `/settings/billing` | `src/app/(app)/settings/billing/page.tsx` |

## Modals and components
| User shorthand or UI label | Component path | Responsibility |
| :--- | :--- | :--- |
| "checkout drawer", "cart", "Buy Now button" | `src/features/checkout/CheckoutDrawer.tsx` | Slide-over cart, quantity selector, and order summary |
| "payment modal", "pay step" | `src/features/checkout/PaymentModal.tsx` | Payment method selection, phone and email validation, and submit |
| "template picker", "browse templates" | `src/features/templates/TemplateGallerySheet.tsx` | Searchable bottom sheet of community templates |
| "onboarding wizard", "signup steps" | `src/features/onboarding/OnboardingWizard.tsx` | 4-step account setup wizard and step gating |

## State stores and hooks
| Shorthand | File path | Responsibility |
| :--- | :--- | :--- |
| "editor state", "canvas store" | `src/stores/useEditorStore.ts` | Zustand store holding active design state and undo history |
| "cart state", "pricing logic" | `src/hooks/useCartTotals.ts` | Bundle discounts, shipping tiers, and line-item calculation |

## API routes and webhooks
| Shorthand | File path | Responsibility |
| :--- | :--- | :--- |
| "Stripe webhook", "payment webhook" | `src/app/api/webhooks/stripe/route.ts` | Handles `checkout.session.completed` and order fulfillment |
| "preview generator", "render image API" | `src/app/api/render-preview/route.ts` | Server-side PNG renderer and cloud storage upload |
| "abandoned cart sync", "email sync" | `src/lib/integrations/marketing-sync.ts` | Sends cart abandonment events and preview URLs to email provider |

## Commands
- Dev server: `npm run dev`
- Targeted tests: `npx vitest run <path-to-test>`
- Typecheck: `npx tsc --noEmit`
