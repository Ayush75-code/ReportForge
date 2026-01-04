# Directory Structure

ReportForge follows a clean, organized folder structure using Next.js 14 App Router with the `/src` directory.

---

## Complete File Tree

```
ReportForge/
│
├── AI_AGENT_PROMPT.txt           # AI agent context and rules
├── TASK_PHASES.md                # Development roadmap
├── START_HERE.md                 # Quick start guide
├── PROGRESS.md                   # Task tracking (create when starting)
├── ERRORS.md                     # Bug log (create when starting)
├── README.md                     # Project overview
│
├── .env.local                    # Environment variables (git-ignored)
├── .gitignore
├── package.json
├── next.config.js
├── tailwind.config.js
├── tsconfig.json
├── postcss.config.js
├── components.json               # Shadcn/UI config
│
├── docs/                         # Documentation (DO NOT add code here)
│   ├── 01-SYSTEM-PROMPTS.md
│   ├── 02-OPERATIONAL-PROCEDURES.md
│   ├── 03-PRODUCT-KNOWLEDGE.md
│   ├── 04-INTERACTION-TEMPLATES.md
│   ├── 05-API-SPECIFICATIONS.md
│   ├── 06-PRD-FULL.md
│   ├── 07-MVP-SPECIFICATION.md
│   ├── 08-SYSTEM-ARCHITECTURE.md
│   ├── 09-DIRECTORY-STRUCTURE.md  # This file
│   ├── 10-DESIGN-SYSTEM.md
│   └── 11-MARKET-RESEARCH.md
│
├── scripts/                      # Utility scripts (NOT in /src)
│   ├── README.md
│   ├── fixes/                    # One-time migration scripts
│   │   └── README.md
│   ├── seeds/                    # Database seed scripts
│   │   └── README.md
│   ├── utils/                    # Reusable utility scripts
│   │   └── README.md
│   └── tests/                    # Manual test scripts
│       └── README.md
│
├── public/                       # Static assets
│   ├── favicon.ico
│   ├── logo.svg
│   └── images/
│
├── tmp/                          # Temporary files (git-ignored)
│
└── src/                          # ALL SOURCE CODE GOES HERE
    │
    ├── app/                      # Next.js 14 App Router
    │   │
    │   ├── layout.tsx            # Root layout
    │   ├── page.tsx              # Landing page (marketing)
    │   ├── globals.css           # Global styles
    │   │
    │   ├── (auth)/               # Auth layout group (no sidebar)
    │   │   ├── layout.tsx        # Auth layout (centered card)
    │   │   ├── login/
    │   │   │   └── page.tsx
    │   │   ├── signup/
    │   │   │   └── page.tsx
    │   │   ├── reset-password/
    │   │   │   └── page.tsx
    │   │   └── callback/
    │   │       └── route.ts      # Supabase auth callback
    │   │
    │   ├── (dashboard)/          # Dashboard layout group (with sidebar)
    │   │   ├── layout.tsx        # Dashboard layout with sidebar
    │   │   │
    │   │   ├── dashboard/
    │   │   │   └── page.tsx      # Dashboard home (usage, recent reports)
    │   │   │
    │   │   ├── reports/
    │   │   │   ├── page.tsx      # Reports list
    │   │   │   ├── new/
    │   │   │   │   └── page.tsx  # New report flow (upload CSV)
    │   │   │   └── [id]/
    │   │   │       ├── page.tsx  # Report detail (read-only)
    │   │   │       └── edit/
    │   │   │           └── page.tsx  # Analysis editor ("The Forge")
    │   │   │
    │   │   ├── clients/
    │   │   │   ├── page.tsx      # Clients list
    │   │   │   └── [id]/
    │   │   │       └── page.tsx  # Client detail
    │   │   │
    │   │   └── settings/
    │   │       ├── page.tsx      # Settings overview
    │   │       ├── branding/
    │   │       │   └── page.tsx  # Logo & brand color
    │   │       ├── billing/
    │   │       │   └── page.tsx  # Subscription & usage
    │   │       └── account/
    │   │           └── page.tsx  # Email, password, delete
    │   │
    │   └── api/                  # API Routes
    │       ├── analyze/
    │       │   └── route.ts      # POST - AI analysis
    │       ├── generate-pdf/
    │       │   └── route.ts      # POST - PDF generation
    │       └── stripe/
    │           ├── create-checkout-session/
    │           │   └── route.ts  # POST - Stripe checkout
    │           └── webhook/
    │               └── route.ts  # POST - Stripe webhooks
    │
    ├── components/               # React components
    │   │
    │   ├── ui/                   # Shadcn/UI base components
    │   │   ├── button.tsx
    │   │   ├── card.tsx
    │   │   ├── input.tsx
    │   │   ├── label.tsx
    │   │   ├── textarea.tsx
    │   │   ├── dialog.tsx
    │   │   ├── dropdown-menu.tsx
    │   │   ├── avatar.tsx
    │   │   ├── toast.tsx
    │   │   ├── progress.tsx
    │   │   ├── badge.tsx
    │   │   └── ... (other Shadcn components)
    │   │
    │   ├── layout/               # Layout components
    │   │   ├── Sidebar.tsx
    │   │   ├── Header.tsx
    │   │   ├── UserNav.tsx
    │   │   └── MobileNav.tsx
    │   │
    │   ├── dashboard/            # Dashboard-specific components
    │   │   ├── UsageWidget.tsx
    │   │   ├── RecentReports.tsx
    │   │   └── QuickActions.tsx
    │   │
    │   ├── reports/              # Report-related components
    │   │   ├── UploadZone.tsx
    │   │   ├── ValidationResult.tsx
    │   │   ├── AnalysisEditor.tsx
    │   │   ├── PerformanceScoreBadge.tsx
    │   │   ├── QuickSummaryCard.tsx
    │   │   ├── PersonalNoteCard.tsx
    │   │   ├── WinsSection.tsx
    │   │   ├── ConcernsSection.tsx
    │   │   ├── RecommendationsSection.tsx
    │   │   ├── ReportCard.tsx
    │   │   └── ReportList.tsx
    │   │
    │   ├── clients/              # Client-related components
    │   │   ├── ClientCard.tsx
    │   │   ├── ClientList.tsx
    │   │   ├── AddClientDialog.tsx
    │   │   └── EditClientDialog.tsx
    │   │
    │   ├── pdf/                  # PDF components (@react-pdf/renderer)
    │   │   ├── ReportPDF.tsx     # Main PDF document
    │   │   ├── PDFHeader.tsx
    │   │   ├── PDFScoreBadge.tsx
    │   │   ├── PDFSection.tsx
    │   │   └── PDFFooter.tsx
    │   │
    │   └── shared/               # Shared/common components
    │       ├── EmptyState.tsx
    │       ├── LoadingSpinner.tsx
    │       ├── ErrorMessage.tsx
    │       └── ConfirmDialog.tsx
    │
    ├── lib/                      # Utilities and clients
    │   │
    │   ├── supabase/             # Supabase clients
    │   │   ├── client.ts         # Browser client
    │   │   ├── server.ts         # Server client
    │   │   └── middleware.ts     # Auth middleware
    │   │
    │   ├── openai/               # OpenAI integration
    │   │   ├── client.ts         # OpenAI client
    │   │   └── prompts.ts        # System prompts
    │   │
    │   ├── stripe/               # Stripe integration
    │   │   └── client.ts
    │   │
    │   ├── csv/                  # CSV processing
    │   │   ├── parser.ts         # PapaParse wrapper
    │   │   └── validator.ts      # Column & data validation
    │   │
    │   ├── analysis/             # Analysis utilities
    │   │   └── aggregator.ts     # Aggregate CSV for AI
    │   │
    │   ├── utils/                # General utilities
    │   │   ├── formatCurrency.ts
    │   │   ├── formatDate.ts
    │   │   └── cn.ts             # Tailwind class merge
    │   │
    │   └── validators/           # Zod schemas
    │       ├── report.ts
    │       ├── client.ts
    │       └── auth.ts
    │
    ├── hooks/                    # Custom React hooks
    │   ├── useAuth.ts
    │   ├── useAgency.ts
    │   ├── useReports.ts
    │   └── useClients.ts
    │
    ├── types/                    # TypeScript types
    │   ├── index.ts              # Re-exports
    │   ├── supabase.ts           # Generated Supabase types
    │   ├── report.ts
    │   ├── client.ts
    │   ├── agency.ts
    │   └── subscription.ts
    │
    └── styles/                   # Additional styles (if needed)
        └── fonts.css             # Custom font imports
```

---

## Folder Purposes

| Folder | Purpose |
|:---|:---|
| `/docs` | Documentation only - no code |
| `/scripts` | Utility scripts, seeds, fixes - run manually |
| `/public` | Static assets served at root URL |
| `/tmp` | Temporary files - git ignored |
| `/src/app` | Next.js pages and API routes |
| `/src/components` | React components organized by feature |
| `/src/lib` | Utilities, clients, helpers |
| `/src/hooks` | Custom React hooks |
| `/src/types` | TypeScript type definitions |

---

## Component Organization

### By Feature
Components are organized by feature area:
- `components/reports/` - All report-related UI
- `components/clients/` - All client-related UI
- `components/pdf/` - PDF generation components
- `components/layout/` - Sidebar, header, navigation

### Shadcn/UI
Base UI components from Shadcn go in `components/ui/`:
- Installed via: `npx shadcn-ui@latest add button`
- DO NOT modify these directly unless necessary

### Naming
- **Components:** PascalCase (`ReportCard.tsx`)
- **Utilities:** camelCase (`formatCurrency.ts`)
- **Hooks:** camelCase with "use" prefix (`useAuth.ts`)

---

## Import Aliases

Configured in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

**Usage:**
```typescript
import { Button } from "@/components/ui/button"
import { supabase } from "@/lib/supabase/client"
import type { Report } from "@/types"
```

---

## File Naming Summary

| Type | Convention | Example |
|:---|:---|:---|
| React components | PascalCase | `ReportCard.tsx` |
| Pages | lowercase | `page.tsx` |
| API routes | lowercase | `route.ts` |
| Utilities | camelCase | `formatDate.ts` |
| Hooks | camelCase | `useAuth.ts` |
| Types | camelCase | `report.ts` |
| Scripts | kebab-case | `seed-clients.ts` |
| Docs | UPPERCASE + number | `06-PRD-FULL.md` |
