# MVP Specification

## 13. MVP SCOPE DEFINITION

### What is MVP?
The Minimum Viable Product (MVP) is the smallest version of ReportForge that delivers core value: transforming Meta Ads CSV exports into branded PDF reports with AI-generated insights in under 60 seconds.

### MVP Philosophy
"Do One Thing Exceptionally Well"
1.  One advertising platform (Meta Ads) only.
2.  One report type (executive summary) only.
3.  One output format (PDF) only.
4.  No charts/graphs (text + numbers only).
5.  No team features (single user accounts).
6.  No integrations (standalone tool).

### Why This Approach?
*   **Faster to market:** Ship in 60 days vs 6 months.
*   **Lower risk:** Validate core value prop before expanding.
*   **Focused feedback:** Users test one workflow, give clear feedback.
*   **Technical simplicity:** Fewer moving parts = fewer bugs.

## 14. MVP FEATURE SET

### MVP Features (MUST HAVE)
| Feature | Description | Complexity | Est. Time |
| :--- | :--- | :--- | :--- |
| **User Authentication** | Email/password signup and login. | Low | 3 days |
| **Agency Branding** | Upload logo, set brand color. | Low | 2 days |
| **Client Management** | Add/edit/delete clients. | Low | 3 days |
| **CSV Upload** | Drag-drop Meta Ads CSV files. | Medium | 4 days |
| **CSV Validation** | Validate required columns + data types. | Medium | 5 days |
| **AI Analysis** | Generate insights using OpenAI API. | High | 7 days |
| **Analysis Editing** | Edit AI text before PDF generation. | Medium | 3 days |
| **PDF Generation** | Create branded PDF with analysis. | Medium | 5 days |
| **Report History** | List past reports, download PDFs. | Low | 3 days |
| **Usage Tracking** | Track reports used, enforce limits. | Medium | 4 days |
| **Subscription (Stripe)** | Trial, paid plans, payment management. | High | 7 days |
| **Settings** | Update account, branding, billing. | Low | 2 days |

**Total Development Time:** ~48 days (with buffer: 60 days)

### MVP User Stories (Prioritized)

**P0 (Critical - Cannot launch without):**
*   ✅ As a new user, I can sign up with email/password so I can create an account.
*   ✅ As a user, I can upload my agency logo so my reports are branded.
*   ✅ As a user, I can add a client so I can organize reports by client.
*   ✅ As a user, I can upload a Meta Ads CSV so I can generate a report.
*   ✅ As a user, I see clear error messages if my CSV is invalid so I can fix it.
*   ✅ As a user, I can generate AI analysis so I don't manually write insights.
*   ✅ As a user, I can download a branded PDF so I can send it to my client.
*   ✅ As a user, I can see my usage so I know how many reports I have left.
*   ✅ As a user, I can subscribe to a paid plan so I can continue using the product after trial.

**P1 (High priority - Should have in MVP):**
*   ✅ As a user, I can edit AI analysis so I can correct any mistakes.
*   ✅ As a user, I can view past reports so I can re-download or reference them.
*   ✅ As a user, I can cancel my subscription so I control my costs.

**P2 (Nice to have - Defer if time-constrained):**
*   As a user, I can regenerate analysis if I'm unsatisfied.
*   As a user, I can filter reports by client.
*   As a user, I can receive email when my report is ready.

## 15. MVP SUCCESS CRITERIA

### Launch Readiness Criteria
**Functional Criteria:**
*   ✅ 100% of P0 user stories complete and tested.
*   ✅ All critical bugs resolved (P0/P1).
*   ✅ End-to-end happy path tested.
*   ✅ Payment flow tested with real Stripe account.
*   ✅ OpenAI API integrated and tested with 20+ sample CSVs.

**Quality Criteria:**
*   ✅ CSV validation accuracy: 95%+.
*   ✅ AI analysis quality: 3.5/5+ average rating.
*   ✅ Report generation success rate: 95%+.
*   ✅ Page load times: <3 seconds.
*   ✅ Mobile responsive (works on tablet).

**Business Criteria:**
*   ✅ 20 beta users signed up and actively testing.
*   ✅ At least 5 beta users willing to pay.
*   ✅ Terms of Service and Privacy Policy finalized.
*   ✅ Stripe account approved.
*   ✅ Support email monitored.

### Post-Launch Success Metrics (First 90 Days)
*   **Trial Signups:** 150
*   **Trial → Paid Conversion:** 10% (15 paying customers)
*   **MRR:** $1,500
*   **Reports Generated:** 500 total
*   **Average Reports per User:** 8-10
*   **Customer Satisfaction:** 4.0+/5
*   **Churn Rate:** <15%

## 16. WHAT'S NOT IN MVP

### Features Explicitly Deferred
**Advertising Platforms:**
*   Google Ads support → Q2 2026
*   LinkedIn Ads support → Q3 2026

**Report Features:**
*   Charts/graphs in PDF → Q2 2026
*   Multiple report templates → Q2 2026
*   Custom sections → Q3 2026
*   Historical trend analysis → Q3 2026

**Collaboration:**
*   Multi-user accounts → Q4 2026
*   Team permissions → Q4 2026

**Integrations:**
*   Zapier integration → Q3 2026
*   CRM integrations → Q4 2026

### Technical Debt Accepted for MVP
*   No charts in PDFs (Text-only reports).
*   Single-user accounts (Simpler DB).
*   CSV-only (No API integrations).
*   Basic error handling.
*   Limited test coverage (Focus on happy path).
*   Manual onboarding.
*   Basic analytics.
*   No caching.

## 17. MVP TIMELINE & MILESTONES

### 60-Day Development Plan

**Week 1-2 (Days 1-14): Foundation**
*   Setup project infrastructure
*   Database schema design + implementation
*   Authentication system
*   Basic UI shell
*   **Milestone:** Can sign up and log in.

**Week 3-4 (Days 15-28): Core Upload Flow**
*   CSV upload component
*   CSV parsing + validation logic
*   Client management CRUD
*   Agency branding settings
*   **Milestone:** Can upload CSV and see validation results.

**Week 5-6 (Days 29-42): AI Integration**
*   OpenAI API integration
*   AI prompt engineering
*   Analysis display + editing UI
*   **Milestone:** AI generates insights from downloaded data.

**Week 7 (Days 43-49): PDF Generation**
*   PDF layout implementation (react-pdf)
*   Branding integration
*   Supabase Storage integration
*   Download flow
*   **Milestone:** Can download branded PDF report.

**Week 8 (Days 50-56): Billing + Polish**
*   Stripe integration
*   Usage tracking + limits
*   Report history page
*   Bug fixes
*   **Milestone:** Full end-to-end flow working.

**Week 9 (Days 57-60): Beta Launch Prep**
*   Final testing (QA)
*   Beta user recruitment
*   Documentation
*   Deploy to production
*   **Milestone: BETA LAUNCH 🚀**
