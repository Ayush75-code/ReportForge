# Directory Structure

## 24. COMPLETE FILE TREE

```
insightengine/
├── .github/
│   └── workflows/
│       ├── deploy.yml                 # CI/CD pipeline
│       ├── test.yml                   # Automated testing
│       └── security-scan.yml          # Vulnerability scanning
│
├── app/                               # Next.js 14 App Router
│   ├── (auth)/                        # Auth layout group
│   │   ├── login/page.tsx
│   │   ├── signup/page.tsx
│   │   └── reset-password/page.tsx
│   │
│   ├── (dashboard)/                   # Dashboard layout group
│   │   ├── layout.tsx                # Sidebar & Nav
│   │   ├── page.tsx                  # Dashboard Home
│   │   ├── clients/                  # Client Management
│   │   │   ├── page.tsx              # List
│   │   │   └── [id]/page.tsx         # Detail
│   │   ├── reports/                  # Report Management
│   │   │   ├── page.tsx              # List
│   │   │   ├── new/page.tsx          # Upload/Create
│   │   │   └── [id]/page.tsx         # Detail/Download
│   │   └── settings/
│   │       ├── branding/page.tsx     # Logo & Colors
│   │       └── billing/page.tsx      # Stripe Portal
│   │
│   ├── api/                           # API Routes
│   │   ├── reports/
│   │   │   ├── upload/route.ts       # POST CSV
│   │   │   ├── [id]/analyze/route.ts # POST AI Trigger
│   │   │   └── [id]/pdf/route.ts     # POST Generate PDF
│   │   └── webhooks/
│   │       └── stripe/route.ts
│   │
│   └── layout.tsx                     # Root Layout
│
├── components/
│   ├── ui/                           # Shadcn UI (Button, Card, etc.)
│   ├── reports/
│   │   ├── upload-zone.tsx           # Drag & Drop
│   │   ├── analysis-editor.tsx       # AI Text Editor
│   │   └── pdf-viewer.tsx            # Preview
│   └── layout/
│       ├── sidebar.tsx
│       └── user-nav.tsx
│
├── lib/
│   ├── supabase/                     # DB Clients
│   │   ├── client.ts                 # Browser Client
│   │   └── server.ts                 # Server Client
│   ├── openai/                       # AI Service
│   │   └── client.ts
│   ├── utils.ts                      # Helpers
│   └── validators.ts                 # Zod Schemas
│
├── public/                           # Static Assets
└── docs/                             # Documentation
```
