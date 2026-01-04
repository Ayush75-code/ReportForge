# ReportForge

**AI-powered reports, forged in seconds.**

ReportForge (formerly InsightEngine) is a B2B SaaS application designed to transform raw Meta Ads CSV exports into professional, branded PDF reports for marketing agencies. It acts as an AI Writing Assistant, replacing hours of manual data analysis and formatting with a streamlined "First Draft" workflow.

## 🚀 Quick Start

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/StartYourBuild/reportforge.git
    cd reportforge
    ```

2.  **Install dependencies:**
    ```bash
    npm install
    ```

3.  **Set up Environment Variables:**
    Copy `.env.local.example` to `.env.local` and add your Supabase, OpenAI, and Stripe keys.

4.  **Run Development Server:**
    ```bash
    npm run dev
    ```

## 📚 Documentation

Detailed documentation is available in the [`/docs`](./docs) directory:

- [System Prompts & AI Logic](./docs/01-SYSTEM-PROMPTS.md)
- [Operational Procedures](./docs/02-OPERATIONAL-PROCEDURES.md)
- [Product Knowledge & FAQs](./docs/03-PRODUCT-KNOWLEDGE.md)
- [API Specifications](./docs/05-API-SPECIFICATIONS.md)
- [Full PRD](./docs/06-PRD-FULL.md)
- [System Architecture](./docs/08-SYSTEM-ARCHITECTURE.md)

## 🛠 Tech Stack

- **Frontend:** Next.js 14 (App Router), Tailwind CSS, shadcn/ui
- **Backend:** Next.js Server Actions, Supabase (PostgreSQL + Auth)
- **AI:** OpenAI GPT-4o-mini
- **PDF Generation:** @react-pdf/renderer
- **Infrastructure:** Vercel (Web), Supabase (DB/Storage)

## 📄 License

Proprietary software. Copyright © 2026 ReportForge. All rights reserved.
