13. MVP SCOPE DEFINITION
What is MVP?
The Minimum Viable Product (MVP) is the smallest version of InsightEngine that delivers core value: transforming Meta Ads CSV exports into branded PDF reports with AI-generated insights in under 60 seconds.
MVP Philosophy
"Do One Thing Exceptionally Well"

One advertising platform (Meta Ads) only
One report type (executive summary) only
One output format (PDF) only
No charts/graphs (text + numbers only)
No team features (single user accounts)
No integrations (standalone tool)

Why This Approach?

Faster to market: Ship in 60 days vs 6 months
Lower risk: Validate core value prop before expanding
Focused feedback: Users test one workflow, give clear feedback
Technical simplicity: Fewer moving parts = fewer bugs


14. MVP FEATURE SET
MVP Features (MUST HAVE)
FeatureDescriptionComplexityTime EstimateUser AuthenticationEmail/password signup and loginLow3 daysAgency BrandingUpload logo, set brand colorLow2 daysClient ManagementAdd/edit/delete clientsLow3 daysCSV UploadDrag-drop Meta Ads CSV filesMedium4 daysCSV ValidationValidate required columns + data typesMedium5 daysAI AnalysisGenerate insights using OpenAI APIHigh7 daysAnalysis EditingEdit AI text before PDF generationMedium3 daysPDF GenerationCreate branded PDF with analysisMedium5 daysReport HistoryList past reports, download PDFsLow3 daysUsage TrackingTrack reports used, enforce limitsMedium4 daysSubscription (Stripe)Trial, paid plans, payment managementHigh7 daysSettingsUpdate account, branding, billingLow2 days
Total Development Time: ~48 days (with buffer: 60 days)

MVP User Stories (Prioritized)
P0 (Critical - Cannot launch without):

✅ As a new user, I can sign up with email/password so I can create an account
✅ As a user, I can upload my agency logo so my reports are branded
✅ As a user, I can add a client so I can organize reports by client
✅ As a user, I can upload a Meta Ads CSV so I can generate a report
✅ As a user, I see clear error messages if my CSV is invalid so I can fix it
✅ As a user, I can generate AI analysis so I don't manually write insights
✅ As a user, I can download a branded PDF so I can send it to my client
✅ As a user, I can see my usage so I know how many reports I have left
✅ As a user, I can subscribe to a paid plan so I can continue using the product after trial

P1 (High priority - Should have in MVP):

✅ As a user, I can edit AI analysis so I can correct any mistakes
✅ As a user, I can view past reports so I can re-download or reference them
✅ As a user, I can cancel my subscription so I control my costs

P2 (Nice to have - Defer if time-constrained):

As a user, I can regenerate analysis if I'm unsatisfied (may defer to post-MVP)
As a user, I can filter reports by client (may defer to post-MVP)
As a user, I can receive email when my report is ready (may defer to post-MVP)


15. MVP SUCCESS CRITERIA
Launch Readiness Criteria
Before launching to public, MVP must meet:
Functional Criteria:

✅ 100% of P0 user stories complete and tested
✅ All critical bugs resolved (P0/P1)
✅ End-to-end happy path tested (signup → report download)
✅ Payment flow tested with real Stripe account
✅ OpenAI API integrated and tested with 20+ sample CSVs

Quality Criteria:

✅ CSV validation accuracy: 95%+ (rejects true errors, accepts valid files)
✅ AI analysis quality: 3.5/5+ average rating (beta tester feedback)
✅ Report generation success rate: 95%+ (completes without errors)
✅ Page load times: <3 seconds (all pages)
✅ Mobile responsive (works on tablet, even if not optimal)

Business Criteria:

✅ 20 beta users signed up and actively testing
✅ At least 5 beta users willing to pay (validation of value prop)
✅ Terms of Service and Privacy Policy finalized
✅ Stripe account approved and tested
✅ Support email (support@insightengine.com) monitored


Post-Launch Success Metrics (First 90 Days)
MetricTargetMeasured HowTrial Signups150Google AnalyticsTrial → Paid Conversion10% (15 paying customers)Stripe DashboardMRR$1,500Stripe DashboardReports Generated500 totalDatabase queryAverage Reports per User8-10Database queryCustomer Satisfaction4.0+/5Post-report surveyChurn Rate<15%Stripe + manual trackingSupport Tickets<20% of usersHelp desk system
If targets are met: Proceed with Q2 roadmap (Google Ads support)
If targets are missed: Iterate on MVP based on feedback before expanding

16. WHAT'S NOT IN MVP
Features Explicitly Deferred to Post-MVP
❌ Not in MVP:
Advertising Platforms:

Google Ads support → Q2 2024
LinkedIn Ads support → Q3 2024
TikTok Ads support → Q4 2024

Report Features:

Charts/graphs in PDF → Q2 2024
Multiple report templates → Q2 2024
Custom sections → Q3 2024
Historical trend analysis → Q3 2024
Benchmark comparisons → Q4 2024

Collaboration:

Multi-user accounts → Q4 2024
Team permissions → Q4 2024
Comments/annotations → Q4 2024
Report approval workflows → 2025

Integrations:

API access → Q3 2024
Zapier integration → Q3 2024
CRM integrations (HubSpot, Salesforce) → Q4 2024
Native Meta/Google Ads API integration → 2025

Distribution:

Email reports directly to clients → Q2 2024
Scheduled report generation → Q3 2024
Client portal (clients log in) → 2025
Mobile app → 2025

Advanced Features:

White-label reseller program → Q4 2024
Custom AI training → 2025
Video report summaries → 2025
Multilingual support → 2025


Technical Debt Accepted for MVP
We're consciously accepting these shortcuts to ship faster:

No charts in PDFs: Text-only reports (easier to generate)
Single-user accounts: No team features (simpler database schema)
CSV-only: No API integrations (avoids OAuth complexity)
Basic error handling: Some edge cases may not be handled gracefully
Limited test coverage: Focus on happy path, not every edge case
Manual onboarding: Founder personally onboards first 100 users
Basic analytics: Google Analytics only (no custom event tracking yet)
No caching: May be slower under load (acceptable for <100 users)

Post-MVP Refactoring Plan:

Add comprehensive test suite (Q2)
Implement caching layer (Q2)
Improve error handling for edge cases (Q3)
Add performance monitoring (Datadog/New Relic) (Q3)


17. MVP TIMELINE & MILESTONES
60-Day Development Plan
Week 1-2 (Days 1-14): Foundation
├─ Setup project infrastructure
├─ Database schema design + implementation
├─ Authentication system (Supabase Auth)
├─ Basic UI shell (navigation, routing)
└─ Milestone: Can sign up and log in

Week 3-4 (Days 15-28): Core Upload Flow
├─ CSV upload component
├─ CSV parsing + validation logic
├─ Client management CRUD
├─ Agency branding settings
└─ Milestone: Can upload CSV and see validation results

Week 5-6 (Days 29-42): AI Integration
├─ OpenAI API integration
├─ AI prompt engineering + testing
├─ Analysis display + editing UI
├─ Error handling for AI failures
└─ Milestone: AI generates insights from uploaded data

Week 7 (Days 43-49): PDF Generation
├─ PDF layout implementation (react-pdf)
├─ Branding integration (logo, colors)
├─ Supabase Storage integration
├─ Download flow
└─ Milestone: Can download branded PDF report

Week 8 (Days 50-56): Billing + Polish
├─ Stripe integration (checkout, webhooks)
├─ Usage tracking + limits
├─ Report history page
├─ Bug fixes + polish
└─ Milestone: Full end-to-end flow working

Week 9 (Days 57-60): Beta Launch Prep
├─ Final testing (QA)
├─ Beta user recruitment
├─ Documentation (help docs, FAQs)
├─ Deploy to production
└─ Milestone: BETA LAUNCH 🚀

Milestones with Acceptance Criteria
Milestone 1: Foundation (Day 14)

✅ User can sign up with email/password
✅ User can log in and log out
✅ Basic dashboard shows (even if empty)
✅ Navigation works (all main pages accessible)
✅ Database schema deployed to Supabase

Milestone 2: Upload Flow (Day 28)

✅ User can add a client
✅ User can upload CSV file (drag-drop works)
✅ System validates CSV and shows errors if invalid
✅ Valid CSV shows summary (campaign count, spend, etc.)
✅ User can upload logo and set brand color

Milestone 3: AI Integration (Day 42)

✅ User can click "Generate Analysis" and see AI output
✅ AI output includes summary, wins, concerns, recommendations
✅ User can edit any section of analysis
✅ Error handling works (shows message if AI fails)
✅ Loading states show during processing

Milestone 4: PDF Generation (Day 49)

✅ User can click "Generate PDF" and download file
✅ PDF includes agency logo and brand color
✅ PDF content matches edited analysis
✅ PDF is properly formatted (no overlapping text, etc.)
✅ Download link persists (can re-download later)

Milestone 5: Billing + Polish (Day 56)

✅ User can subscribe to paid plan via Stripe
✅ Usage tracking shows correctly
✅ Trial limits are enforced
✅ User can view past reports and download PDFs
✅ Major bugs fixed, UI polished

Milestone 6: Beta Launch (Day 60)

✅ All P0 user stories complete
✅ 10+ beta users actively testing
✅ Help documentation published
✅ Support email monitored
✅ Public landing page live


Risk Management
High-Risk Items:
RiskLikelihoodImpactMitigationOpenAI API reliability issuesMediumHighBuild retry logic + manual fallbackPDF generation performance slowMediumMediumOptimize early, use cachingStripe integration complexityLowHighUse Stripe prebuilt componentsBeta user recruitment failsMediumMediumStart outreach in Week 4Scope creepHighHighStrict "no new features" policy

PART 3: SYSTEM ARCHITECTURE

18.HIGH-LEVEL ARCHITECTURE
Architecture Overview Diagram
┌──────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                           │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │             Next.js 14 (App Router)                   │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐     │   │
│  │  │  React UI  │  │   TailwindCSS │  │ shadcn/ui  │   │   │
│  │  └────────────┘  └────────────┘  └────────────┘     │   │
│  │                                                       │   │
│  │  Features:                                           │   │
│  │  • Server Components (RSC)                           │   │
│  │  • Client Components (interactive UI)               │   │
│  │  • Server Actions (form handling)                    │   │
│  │  • API Routes (REST endpoints)                       │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                               │
└───────────────────────┬───────────────────────────────────────┘
                        │
                        │ HTTPS/TLS 1.3
                        │
┌───────────────────────▼───────────────────────────────────────┐
│                     APPLICATION LAYER                          │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │          Next.js API Routes (Serverless)             │   │
│  │                                                       │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌───────────┐ │   │
│  │  │ CSV Handler  │  │ AI Service   │  │ PDF Gen   │ │   │
│  │  └──────────────┘  └──────────────┘  └───────────┘ │   │
│  │                                                       │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌───────────┐ │   │
│  │  │ Auth Handler │  │ Billing Svc  │  │ Webhook   │ │   │
│  │  └──────────────┘  └──────────────┘  └───────────┘ │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                               │
└───────────┬───────────────────┬───────────────┬──────────────┘
            │                   │               │
            │                   │               │
┌───────────▼────────┐ ┌────────▼──────┐ ┌────▼──────────────┐
│   DATA LAYER       │ │ EXTERNAL APIs │ │  STORAGE LAYER    │
├────────────────────┤ ├───────────────┤ ├───────────────────┤
│                    │ │               │ │                   │
│  ┌──────────────┐ │ │ ┌───────────┐ │ │ ┌───────────────┐│
│  │  Supabase    │ │ │ │  OpenAI   │ │ │ │   Supabase    ││
│  │  PostgreSQL  │ │ │ │  GPT-4o   │ │ │ │   Storage     ││
│  │              │ │ │ │           │ │ │ │               ││
│  │  Tables:     │ │ │ └───────────┘ │ │ │  Buckets:     ││
│  │  • agencies  │ │ │               │ │ │  • reports/   ││
│  │  • clients   │ │ │ ┌───────────┐ │ │ │    (PDFs)     ││
│  │  • reports   │ │ │ │  Stripe   │ │ │ │  • logos/     ││
│  │  • auth      │ │ │ │  API      │ │ │ │    (branding) ││
│  └──────────────┘ │ │ └───────────┘ │ │ └───────────────┘│
│                    │ │               │ │                   │
│  ┌──────────────┐ │ │ ┌───────────┐ │ │ ┌───────────────┐│
│  │ Row Level    │ │ │ │ Postmark  │ │ │ │   CDN Cache   ││
│  │ Security     │ │ │ │ Email API │ │ │ │   (Vercel)    ││
│  │ (RLS)        │ │ │ └───────────┘ │ │ └───────────────┘│
│  └──────────────┘ │ │               │ │                   │
└────────────────────┘ └───────────────┘ └───────────────────┘

Architecture Patterns
1. Serverless Architecture

Next.js API routes deploy as serverless functions (Vercel Edge Functions)
Auto-scaling based on demand
Pay-per-execution pricing
Cold start optimized (<1 second)

2. Event-Driven Processing

Report generation is asynchronous
Status updates via polling (MVP) or webhooks (post-MVP)
Stripe webhooks for payment events

3. Multi-Tenant SaaS

Single application instance serves all agencies
Data isolation via Row Level Security (RLS)
Agency ID is the tenant identifier

4. API-First Design

All features accessible via internal APIs
Enables future public API release (Q3 2024)
Frontend consumes same APIs

5. Stateless Services

No session state stored on servers
JWT tokens for authentication
Enables horizontal scaling


Data Flow: Report Generation
┌─────────────┐
│   Browser   │
└──────┬──────┘
       │
       │ 1. Upload CSV
       │
       ▼
┌──────────────────────┐
│  /api/reports/upload │
└──────┬───────────────┘
       │
       │ 2. Parse & Validate
       │
       ▼
┌──────────────────────┐     3. Store CSV
│  CSV Validator       ├────────────────────┐
└──────┬───────────────┘                    │
       │                                     ▼
       │ 4. Create report record    ┌──────────────┐
       │                             │   Supabase   │
       ▼                             │   Storage    │
┌──────────────────────┐            └──────────────┘
│   PostgreSQL         │
│   reports table      │
│   status='draft'     │
└──────┬───────────────┘
       │
       │ 5. User clicks "Generate Analysis"
       │
       ▼
┌──────────────────────┐
│ /api/reports/analyze │
└──────┬───────────────┘
       │
       │ 6. Update status='processing'
       │
       ▼
┌──────────────────────┐
│  PostgreSQL          │
└──────┬───────────────┘
       │
       │ 7. Fetch parsed data
       │
       ▼
┌──────────────────────┐
│  AI Analysis Service │
└──────┬───────────────┘
       │
       │ 8. Call OpenAI API
       │
       ▼
┌──────────────────────┐
│   OpenAI GPT-4o-mini │
└──────┬───────────────┘
       │
       │ 9. Return analysis JSON
       │
       ▼
┌──────────────────────┐
│  AI Analysis Service │
└──────┬───────────────┘
       │
       │ 10. Store analysis
       │     status='completed'
       │
       ▼
┌──────────────────────┐
│   PostgreSQL         │
│   reports.ai_analysis│
└──────┬───────────────┘
       │
       │ 11. User clicks "Generate PDF"
       │
       ▼
┌──────────────────────┐
│ /api/reports/pdf     │
└──────┬───────────────┘
       │
       │ 12. Fetch report + branding
       │
       ▼
┌──────────────────────┐
│  PDF Generator       │
│  (react-pdf)         │
└──────┬───────────────┘
       │
       │ 13. Render PDF buffer
       │
       ▼
┌──────────────────────┐
│  Supabase Storage    │
│  upload PDF          │
└──────┬───────────────┘
       │
       │ 14. Store public URL
       │
       ▼
┌──────────────────────┐
│   PostgreSQL         │
│   reports.pdf_url    │
└──────┬───────────────┘
       │
       │ 15. Return download URL
       │
       ▼
┌──────────────────────┐
│      Browser         │
│   [Download PDF]     │
└──────────────────────┘

19. TECHNOLOGY STACK
Frontend Stack
TechnologyVersionPurposeWhy ChosenNext.js14.xReact frameworkServer components, API routes, built-in routingReact18.xUI libraryComponent-based, large ecosystemTypeScript5.xType safetyCatch bugs at compile time, better DXTailwind CSS3.xStylingUtility-first, fast development, small bundleshadcn/uiLatestComponent libraryAccessible, customizable, Tailwind-basedLucide ReactLatestIconsLightweight, consistent designReact Hook Form7.xForm handlingPerformance, validation, less re-rendersZod3.xSchema validationType-safe validation, integrates with RHFPapaParse5.xCSV parsingRobust, handles edge cases, widely used@react-pdf/renderer3.xPDF generationReact-based, no headless browser needed

Backend Stack
TechnologyVersionPurposeWhy ChosenNext.js API Routes14.xAPI endpointsServerless, same codebase as frontendSupabaseLatestDatabase + Auth + StorageAll-in-one, PostgreSQL, RLS built-inPostgreSQL15.xDatabaseJSONB support, robust, scalableOpenAI APILatestAI analysisGPT-4o-mini for cost efficiencyStripeLatestPaymentsIndustry standard, great docs, webhooksPostmarkLatestTransactional emailReliable, great deliverability

Infrastructure & DevOps
TechnologyPurposeWhy ChosenVercelHostingNext.js optimized, edge functions, zero configSupabase CloudDatabase hostingManaged PostgreSQL, automated backupsGitHubVersion controlStandard, integrates with VercelGitHub ActionsCI/CDFree for public repos, native Git integrationSentryError trackingReal-time alerts, stack traces, performance monitoringVercel AnalyticsWeb analyticsBuilt-in, privacy-friendly, no cookies

Development Tools
ToolPurposeVS CodeCode editorESLintCode lintingPrettierCode formattingHuskyGit hooks (pre-commit checks)JestUnit testingPlaywrightE2E testingPostmanAPI testing

Third-Party Services
ServicePurposeCost (Estimated)Vercel ProHosting$20/monthSupabase ProDatabase + Storage$25/monthOpenAI APIAI analysis$200-500/month (scales with usage)StripePayment processing2.9% + $0.30 per transactionPostmarkEmail$10/month (10K emails)SentryError trackingFree tier (5K events/month)GitHubRepo hostingFreeDomaininsightengine.com$12/year
Total Infrastructure Cost: ~$300-600/month

20. DATABASE ARCHITECTURE
Database Schema (Complete)
sql-- Enable UUID extension
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- ============================================================
-- AGENCIES TABLE (Multi-tenant root)
-- ============================================================
CREATE TABLE agencies (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE UNIQUE NOT NULL,
  
  -- Agency info
  agency_name TEXT NOT NULL,
  logo_url TEXT,
  brand_color TEXT DEFAULT '#6366f1' CHECK (brand_color ~ '^#[0-9A-Fa-f]{6}$'),
  
  -- Subscription
  stripe_customer_id TEXT UNIQUE,
  stripe_subscription_id TEXT,
  subscription_status TEXT DEFAULT 'trialing' CHECK (
    subscription_status IN ('trialing', 'active', 'canceled', 'past_due', 'paused')
  ),
  plan_type TEXT DEFAULT 'starter' CHECK (
    plan_type IN ('trial', 'starter', 'pro', 'enterprise')
  ),
  
  -- Usage tracking
  reports_used_this_month INTEGER DEFAULT 0 CHECK (reports_used_this_month >= 0),
  report_limit INTEGER DEFAULT 10, -- 10 for trial, 20 for starter, 100 for pro
  
  -- Timestamps
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  
  -- Metadata
  metadata JSONB DEFAULT '{}'::jsonb
);

-- ============================================================
-- CLIENTS TABLE (Agency's customers)
-- ============================================================
CREATE TABLE clients (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  agency_id UUID REFERENCES agencies(id) ON DELETE CASCADE NOT NULL,
  
  -- Client info
  client_name TEXT NOT NULL,
  website_url TEXT,
  industry TEXT,
  
  -- Timestamps
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  
  -- Constraints
  CONSTRAINT unique_client_per_agency UNIQUE (agency_id, client_name)
);

-- ============================================================
-- REPORTS TABLE (Generated reports)
-- ============================================================
CREATE TABLE reports (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  agency_id UUID REFERENCES agencies(id) ON DELETE CASCADE NOT NULL,
  client_id UUID REFERENCES clients(id) ON DELETE CASCADE NOT NULL,
  
  -- File metadata
  csv_filename TEXT NOT NULL,
  csv_storage_path TEXT, -- Path in Supabase Storage
  
  -- Data
  parsed_data JSONB NOT NULL, -- Array of campaign objects
  
  -- AI analysis
  ai_analysis JSONB, -- {summary, wins, concerns, recommendations}
  
  -- Output
  pdf_url TEXT,
  
  -- Status tracking
  status TEXT DEFAULT 'draft' NOT NULL CHECK (
    status IN ('draft', 'processing', 'completed', 'failed', 'pdf_generation_failed')
  ),
  error_message TEXT,
  
  -- Processing metadata
  processing_started_at TIMESTAMPTZ,
  completed_at TIMESTAMPTZ,
  retry_count INTEGER DEFAULT 0 CHECK (retry_count >= 0 AND retry_count <= 3),
  
  -- Timestamps
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  
  -- Metadata (for analytics, debugging)
  metadata JSONB DEFAULT '{}'::jsonb -- stores: campaign_count, total_spend, date_range, etc.
);

-- ============================================================
-- AUDIT LOG TABLE (Security & debugging)
-- ============================================================
CREATE TABLE audit_log (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  
  -- Who
  agency_id UUID REFERENCES agencies(id) ON DELETE SET NULL,
  user_id UUID REFERENCES auth.users(id) ON DELETE SET NULL,
  
  -- What
  action TEXT NOT NULL, -- 'report_created', 'subscription_upgraded', etc.
  resource_type TEXT NOT NULL, -- 'report', 'agency', 'client'
  resource_id UUID,
  
  -- Context
  ip_address INET,
  user_agent TEXT,
  
  -- Details
  details JSONB DEFAULT '{}'::jsonb,
  
  -- When
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL
);

-- ============================================================
-- INDEXES (Performance optimization)
-- ============================================================

-- Agencies
CREATE INDEX idx_agencies_user_id ON agencies(user_id);
CREATE INDEX idx_agencies_stripe_customer ON agencies(stripe_customer_id) WHERE stripe_customer_id IS NOT NULL;
CREATE INDEX idx_agencies_subscription_status ON agencies(subscription_status);

-- Clients
CREATE INDEX idx_clients_agency_id ON clients(agency_id);
CREATE INDEX idx_clients_name ON clients(client_name);

-- Reports
CREATE INDEX idx_reports_agency_id ON reports(agency_id);
CREATE INDEX idx_reports_client_id ON reports(client_id);
CREATE INDEX idx_reports_status ON reports(status);
CREATE INDEX idx_reports_created_at ON reports(created_at DESC);
CREATE UNIQUE INDEX idx_processing_lock ON reports(id) WHERE status = 'processing'; -- Idempotency

-- Audit Log
CREATE INDEX idx_audit_agency ON audit_log(agency_id);
CREATE INDEX idx_audit_created ON audit_log(created_at DESC);
CREATE INDEX idx_audit_action ON audit_log(action);

-- ============================================================
-- ROW LEVEL SECURITY (RLS)
-- ============================================================

-- Enable RLS on all tables
ALTER TABLE agencies ENABLE ROW LEVEL SECURITY;
ALTER TABLE clients ENABLE ROW LEVEL SECURITY;
ALTER TABLE reports ENABLE ROW LEVEL SECURITY;
ALTER TABLE audit_log ENABLE ROW LEVEL SECURITY;

-- Agencies: Users can only see their own agency
CREATE POLICY "Users can view own agency"
  ON agencies FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "Users can update own agency"
  ON agencies FOR UPDATE
  USING (auth.uid() = user_id);

-- Clients: Users can only see their agency's clients
CREATE POLICY "Users can view own clients"
  ON clients FOR SELECT
  USING (agency_id IN (SELECT id FROM agencies WHERE user_id = auth.uid()));

CREATE POLICY "Users can insert own clients"
  ON clients FOR INSERT
  WITH CHECK (agency_id IN (SELECT id FROM agencies WHERE user_id = auth.uid()));

CREATE POLICY "Users can update own clients"
  ON clients FOR UPDATE
  USING (agency_id IN (SELECT id FROM agencies WHERE user_id = auth.uid()));

CREATE POLICY "Users can delete own clients"
  ON clients FOR DELETE
  USING (agency_id IN (SELECT id FROM agencies WHERE user_id = auth.uid()));

-- Reports: Users can only see their agency's reports
CREATE POLICY "Users can view own reports"
  ON reports FOR SELECT
  USING (agency_id IN (SELECT id FROM agencies WHERE user_id = auth.uid()));

CREATE POLICY "Users can insert own reports"
  ON reports FOR INSERT
  WITH CHECK (agency_id IN (SELECT id FROM agencies WHERE user_id = auth.uid()));

CREATE POLICY "Users can update own reports"
  ON reports FOR UPDATE
  USING (agency_id IN (SELECT id FROM agencies WHERE user_id = auth.uid()));

CREATE POLICY "Users can delete own reports"
  ON reports FOR DELETE
  USING (agency_id IN (SELECT id FROM agencies WHERE user_id = auth.uid()));

-- Audit Log: Users can view their own audit logs only
CREATE POLICY "Users can view own audit logs"
  ON audit_log FOR SELECT
  USING (agency_id IN (SELECT id FROM agencies WHERE user_id = auth.uid()));

-- ============================================================
-- FUNCTIONS & TRIGGERS
-- ============================================================

-- Function: Update updated_at timestamp
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Trigger: Auto-update updated_at on agencies
CREATE TRIGGER update_agencies_updated_at BEFORE UPDATE ON agencies
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- Trigger: Auto-update updated_at on reports
CREATE TRIGGER update_reports_updated_at BEFORE UPDATE ON reports
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- Function: Reset monthly usage (called by cron job)
CREATE OR REPLACE FUNCTION reset_monthly_usage()
RETURNS void AS $$
BEGIN
  UPDATE agencies SET reports_used_this_month = 0;
  
  INSERT INTO audit_log (action, resource_type, details)
  VALUES ('monthly_usage_reset', 'system', jsonb_build_object('reset_date', NOW()));
END;
$$ LANGUAGE plpgsql;

-- ============================================================
-- SAMPLE DATA (For development/testing)
-- ============================================================

-- Insert sample agency (only in development)
-- INSERT INTO agencies (user_id, agency_name, plan_type, report_limit)
-- VALUES (
--   '00000000-0000-0000-0000-000000000000', -- Replace with real user_id
--   'Demo Agency',
--   'pro',
--   100
-- );
```

---

### **Database Relationships Diagram**
```
┌─────────────────────┐
│    auth.users       │ (Supabase Auth)
│  ─────────────────  │
│  • id (UUID)        │
│  • email            │
│  • encrypted_pwd    │
└──────────┬──────────┘
           │
           │ 1:1
           │
┌──────────▼──────────┐
│     agencies        │
│  ─────────────────  │
│  • id (PK)          │
│  • user_id (FK)     │◄────┐
│  • agency_name      │     │
│  • logo_url         │     │
│  • brand_color      │     │
│  • stripe_*         │     │
│  • reports_used     │     │
│  • report_limit     │     │
└──────────┬──────────┘     │
           │                 │
           │ 1:N             │
           │                 │
┌──────────▼──────────┐     │
│      clients        │     │
│  ─────────────────  │     │
│  • id (PK)          │     │
│  • agency_id (FK)   ├─────┘
│  • client_name      │
│  • website_url      │
│  • industry         │
└──────────┬──────────┘
           │
           │ 1:N
           │
┌──────────▼──────────┐
│      reports        │
│  ─────────────────  │
│  • id (PK)          │
│  • agency_id (FK)   │───────┐
│  • client_id (FK)   │       │
│  • csv_filename     │       │
│  • parsed_data      │       │
│  • ai_analysis      │       │
│  • pdf_url          │       │
│  • status           │       │
└─────────────────────┘       │
                               │
                               │ N:1
                               │
                     ┌─────────▼──────────┐
                     │    agencies        │
                     │  (referenced above)│
                     └────────────────────┘
```

---

## **21. API ARCHITECTURE**

### **API Endpoint Structure**
```
/api
├── /auth
│   ├── /signup         POST   Create new user account
│   ├── /login          POST   Authenticate user
│   ├── /logout         POST   End user session
│   └── /reset-password POST   Send password reset email
│
├── /agencies
│   ├── /              GET    Get current agency details
│   ├── /              PATCH  Update agency settings
│   └── /branding      PATCH  Update logo/brand color
│
├── /clients
│   ├── /              GET    List all clients
│   ├── /              POST   Create new client
│   ├── /:id           GET    Get specific client
│   ├── /:id           PATCH  Update client
│   └── /:id           DELETE Delete client
│
├── /reports
│   ├── /              GET    List all reports (paginated, filtered)
│   ├── /upload        POST   Upload CSV and create report draft
│   ├── /:id           GET    Get specific report details
│   ├── /:id/analyze   POST   Generate AI analysis
│   ├── /:id/pdf       POST   Generate PDF from analysis
│   ├── /:id           DELETE Delete report
│   └── /:id/download  GET    Download PDF (redirects to storage URL)
│
├── /usage
│   └── /              GET    Get current usage stats and limits
│
├── /billing
│   ├── /checkout      POST   Create Stripe Checkout session
│   ├── /portal        POST   Create Stripe Customer Portal session
│   └── /webhook       POST   Handle Stripe webhooks
│
└── /health
    └── /              GET    Health check endpoint

API Request/Response Examples
POST /api/reports/upload
Request:
httpPOST /api/reports/upload
Content-Type: multipart/form-data
Authorization: Bearer {jwt_token}

file: {csv_file}
client_id: "550e8400-e29b-41d4-a716-446655440000"
Response (Success - 201 Created):
json{
  "success": true,
  "data": {
    "report_id": "7c9e6679-7425-40de-944b-e07fc1f90ae7",
    "client_name": "Acme Corp",
    "validation": {
      "rows_validated": 12,
      "campaigns": 12,
      "total_spend": 2450.50,
      "total_impressions": 450000,
      "total_conversions": 89,
      "date_range": {
        "start": "2024-01-01",
        "end": "2024-01-31"
      }
    },
    "warnings": [],
    "status": "draft",
    "created_at": "2024-01-15T14:25:00Z"
  }
}
Response (Error - 400 Bad Request):
json{
  "success": false,
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "CSV validation failed",
    "details": {
      "missing_columns": ["Conversions", "CPA"],
      "found_columns": ["Campaign Name", "Impressions", "Clicks", "Spend", "CPC", "CTR"]
    },
    "fix": "Please re-export from Meta Ads Manager and ensure the following columns are included: Conversions, CPA"
  }
}

POST /api/reports/:id/analyze
Request:
httpPOST /api/reports/7c9e6679-7425-40de-944b-e07fc1f90ae7/analyze
Content-Type: application/json
Authorization: Bearer {jwt_token}

{}
Response (Success - 200 OK):
json{
  "success": true,
  "data": {
    "report_id": "7c9e6679-7425-40de-944b-e07fc1f90ae7",
    "status": "completed",
    "analysis": {
      "summary": "Campaigns generated 89 conversions at an average CPA of $27.53, performing 8% above target efficiency. Winter Sale drove 42% of total conversions with exceptional CTR of 3.2%.",
      "wins": [
        "Winter Sale campaign achieved CPA of $18.50, 33% below target - immediate budget increase recommended",
        "CTR of 3.2% on Spring Launch indicates strong creative resonance with target audience",
        "Overall conversion volume increased 22% compared to previous period while maintaining cost efficiency"
      ],
      "concerns": [
        "Summer Campaign spent $450 with zero conversions - pause immediately and audit targeting parameters",
        "Mobile CPA trending 15% higher than desktop across all campaigns - consider mobile-specific creative optimization"
      ],
      "recommendations": [
        "Increase daily budget on Winter Sale campaign by $200 to capture additional high-intent traffic while maintaining current CPA",
        "Pause Summer Campaign and conduct full audit of audience targeting, ad creative, and landing page experience before relaunch",
        "Launch A/B test with mobile-optimized creative variants to address mobile conversion rate gap"
      ]
    },
    "confidence_score": 0.92,
    "processing_time_ms": 18420,
    "completed_at": "2024-01-15T14:32:00Z"
  }
}
Response (Error - 409 Conflict):
json{
  "success": false,
  "error": {
    "code": "ALREADY_PROCESSING",
    "message": "Report is already being analyzed",
    "details": {
      "report_id": "7c9e6679-7425-40de-944b-e07fc1f90ae7",
      "current_status": "processing",
      "started_at": "2024-01-15T14:30:00Z"
    }
  }
}

API Authentication
JWT Token Structure:
json{
  "sub": "user-uuid",
  "email": "sarah@agency.com",
  "agency_id": "agency-uuid",
  "plan_type": "pro",
  "iat": 1705330800,
  "exp": 1705935600
}
Token Validation:

Issued by Supabase Auth
Verified using Supabase JWT secret
Expires after 7 days (refresh token available for 30 days)
Includes agency_id for efficient RLS queries


Rate Limiting
javascript// Rate limit configuration per plan
const RATE_LIMITS = {
  trial: {
    requests_per_hour: 50,
    reports_per_hour: 5
  },
  starter: {
    requests_per_hour: 100,
    reports_per_hour: 10
  },
  pro: {
    requests_per_hour: 500,
    reports_per_hour: 50
  },
  enterprise: {
    requests_per_hour: 2000,
    reports_per_hour: 200
  }
};

// Implementation (middleware)
export async function
// Implementation (middleware)
export async function rateLimitMiddleware(req, userId, planType) {
  const key = `rate_limit:${userId}:${Date.now() / 3600000 | 0}`; // hourly bucket
  
  const current = await redis.incr(key);
  await redis.expire(key, 3600); // expire after 1 hour
  
  const limit = RATE_LIMITS[planType].requests_per_hour;
  
  if (current > limit) {
    throw new RateLimitError({
      code: 'RATE_LIMIT_EXCEEDED',
      message: `Rate limit exceeded. Limit: ${limit} requests/hour`,
      limit: limit,
      reset_at: new Date(Math.ceil(Date.now() / 3600000) * 3600000)
    });
  }
  
  // Add headers to response
  res.setHeader('X-RateLimit-Limit', limit);
  res.setHeader('X-RateLimit-Remaining', limit - current);
  res.setHeader('X-RateLimit-Reset', Math.ceil(Date.now() / 3600000) * 3600000);
  
  return { allowed: true };
}
Response Headers:
httpHTTP/1.1 200 OK
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1705334400
Rate Limit Exceeded Response:
httpHTTP/1.1 429 Too Many Requests
Retry-After: 3600
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1705334400

{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Limit: 100 requests/hour",
    "limit": 100,
    "reset_at": "2024-01-15T15:00:00Z"
  }
}
```

---

## **22. INFRASTRUCTURE & DEPLOYMENT**

### **Hosting Architecture**
```
┌────────────────────────────────────────────────────────────┐
│                      VERCEL DEPLOYMENT                      │
├────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────────────────────────────────────────────┐ │
│  │           Production Environment                     │ │
│  │  ─────────────────────────────────────────────────  │ │
│  │  Domain: insightengine.com                          │ │
│  │  Branch: main                                        │ │
│  │  Deployment: Automatic on push                      │ │
│  │  Edge Network: Global CDN (300+ locations)          │ │
│  │  SSL: Auto-renewed Let's Encrypt                    │ │
│  └──────────────────────────────────────────────────────┘ │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐ │
│  │           Preview Environments                       │ │
│  │  ─────────────────────────────────────────────────  │ │
│  │  Domain: {branch}-insightengine.vercel.app          │ │
│  │  Purpose: PR testing, staging                       │ │
│  │  Deployment: Automatic on PR creation               │ │
│  └──────────────────────────────────────────────────────┘ │
│                                                             │
└────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────┐
│                    SUPABASE DEPLOYMENT                      │
├────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────────────────────────────────────────────┐ │
│  │           Production Database                        │ │
│  │  ─────────────────────────────────────────────────  │ │
│  │  Region: US East (us-east-1)                        │ │
│  │  Instance: Supabase Pro                             │ │
│  │  Compute: Dedicated 2 CPU, 4GB RAM                  │ │
│  │  Storage: 8GB included, auto-scales                 │ │
│  │  Backups: Daily, retained 7 days                    │ │
│  └──────────────────────────────────────────────────────┘ │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐ │
│  │           Development Database                       │ │
│  │  ─────────────────────────────────────────────────  │ │
│  │  Region: US East (us-east-1)                        │ │
│  │  Instance: Supabase Free Tier                       │ │
│  │  Purpose: Local development, testing                │ │
│  └──────────────────────────────────────────────────────┘ │
│                                                             │
└────────────────────────────────────────────────────────────┘

CI/CD Pipeline
GitHub Actions Workflow:
yaml# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run linter
        run: npm run lint
      
      - name: Run type check
        run: npm run type-check
      
      - name: Run unit tests
        run: npm run test:unit
      
      - name: Run integration tests
        run: npm run test:integration
        env:
          DATABASE_URL: ${{ secrets.TEST_DATABASE_URL }}
          OPENAI_API_KEY: ${{ secrets.TEST_OPENAI_API_KEY }}
  
  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build application
        run: npm run build
        env:
          NEXT_PUBLIC_SUPABASE_URL: ${{ secrets.NEXT_PUBLIC_SUPABASE_URL }}
          NEXT_PUBLIC_SUPABASE_ANON_KEY: ${{ secrets.NEXT_PUBLIC_SUPABASE_ANON_KEY }}
      
      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build
          path: .next
  
  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'
      
      - name: Run database migrations
        run: npm run db:migrate
        env:
          DATABASE_URL: ${{ secrets.PROD_DATABASE_URL }}
      
      - name: Notify team on Slack
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: 'Production deployment completed'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

---

### **Deployment Process**

**1. Development → Staging**
```
Developer pushes to feature branch
    ↓
GitHub Actions runs tests
    ↓
If tests pass, deploys to preview environment
    ↓
Preview URL: feature-branch-insightengine.vercel.app
    ↓
QA testing on preview environment
```

**2. Staging → Production**
```
PR merged to main branch
    ↓
GitHub Actions runs full test suite
    ↓
Build production bundle
    ↓
Deploy to Vercel production
    ↓
Run database migrations (if any)
    ↓
Verify health checks pass
    ↓
Notify team on Slack
```

**3. Rollback Procedure**
```
Issue detected in production
    ↓
Go to Vercel dashboard
    ↓
Click "Rollback" on previous deployment
    ↓
Traffic instantly routes to previous version
    ↓
Total rollback time: < 2 minutes

Environment Variables
Production (.env.production):
bash# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://xxxxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJxxx...
SUPABASE_SERVICE_ROLE_KEY=eyJxxx... # Server-side only, never exposed

# OpenAI
OPENAI_API_KEY=sk-xxx...
OPENAI_ORG_ID=org-xxx...

# Stripe
STRIPE_SECRET_KEY=sk_live_xxx...
STRIPE_WEBHOOK_SECRET=whsec_xxx...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_xxx...

# Email
POSTMARK_API_TOKEN=xxx...
POSTMARK_FROM_EMAIL=noreply@insightengine.com

# Application
NEXT_PUBLIC_APP_URL=https://insightengine.com
NODE_ENV=production

# Monitoring
SENTRY_DSN=https://xxx@sentry.io/xxx
SENTRY_AUTH_TOKEN=xxx...

# Feature Flags
NEXT_PUBLIC_ENABLE_API=false # API not available in MVP
NEXT_PUBLIC_ENABLE_GOOGLE_ADS=false # Coming Q2 2024
Development (.env.local):
bash# Supabase (development project)
NEXT_PUBLIC_SUPABASE_URL=https://xxxxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJxxx...
SUPABASE_SERVICE_ROLE_KEY=eyJxxx...

# OpenAI (use test key with lower rate limits)
OPENAI_API_KEY=sk-test-xxx...

# Stripe (test mode)
STRIPE_SECRET_KEY=sk_test_xxx...
STRIPE_WEBHOOK_SECRET=whsec_test_xxx...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_xxx...

# Email (Postmark test server)
POSTMARK_API_TOKEN=POSTMARK_API_TEST

# Application
NEXT_PUBLIC_APP_URL=http://localhost:3000
NODE_ENV=development

# Monitoring (disabled in dev)
SENTRY_DSN=

Monitoring & Alerting
Health Check Endpoint:
typescript// app/api/health/route.ts
export async function GET() {
  const checks = {
    status: 'healthy',
    timestamp: new Date().toISOString(),
    services: {
      database: await checkDatabase(),
      storage: await checkStorage(),
      openai: await checkOpenAI(),
      stripe: await checkStripe()
    }
  };
  
  const allHealthy = Object.values(checks.services).every(s => s.status === 'ok');
  
  return Response.json(checks, {
    status: allHealthy ? 200 : 503
  });
}

async function checkDatabase() {
  try {
    await supabase.from('agencies').select('count').limit(1);
    return { status: 'ok', latency_ms: 45 };
  } catch (error) {
    return { status: 'error', error: error.message };
  }
}

// Similar checks for other services...
Sentry Error Tracking:
typescript// lib/sentry.ts
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 0.1, // 10% of transactions
  beforeSend(event) {
    // Don't send errors from development
    if (process.env.NODE_ENV === 'development') {
      return null;
    }
    return event;
  }
});
```

**Uptime Monitoring:**
- Service: UptimeRobot (free tier)
- Checks: Every 5 minutes
- Endpoints monitored:
  - https://insightengine.com (main site)
  - https://insightengine.com/api/health
- Alerts: Email + Slack on downtime

**Performance Monitoring:**
- Vercel Analytics (built-in)
- Metrics tracked:
  - Core Web Vitals (LCP, FID, CLS)
  - API endpoint response times
  - Error rates by endpoint

---

## **23. SECURITY ARCHITECTURE**

### **Security Layers**
```
┌─────────────────────────────────────────────────────────┐
│                    LAYER 1: NETWORK                      │
├─────────────────────────────────────────────────────────┤
│  • TLS 1.3 encryption (all traffic)                     │
│  • HTTPS only (HTTP redirects to HTTPS)                 │
│  • DDoS protection (Vercel)                             │
│  • Rate limiting per IP                                 │
└─────────────────────────────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│                LAYER 2: AUTHENTICATION                   │
├─────────────────────────────────────────────────────────┤
│  • JWT tokens (Supabase Auth)                           │
│  • Password hashing (bcrypt, 10 rounds)                 │
│  • Email verification required                          │
│  • Password requirements: 8+ chars, mixed case          │
│  • Session timeout: 7 days                              │
│  • Refresh tokens: 30 days                              │
└─────────────────────────────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│                LAYER 3: AUTHORIZATION                    │
├─────────────────────────────────────────────────────────┤
│  • Row Level Security (RLS) in database                 │
│  • Agency ID scoping on all queries                     │
│  • Service accounts for background jobs                 │
│  • API key rotation (90 days)                           │
└─────────────────────────────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│                  LAYER 4: DATA SECURITY                  │
├─────────────────────────────────────────────────────────┤
│  • Encryption at rest (AES-256)                         │
│  • Encryption in transit (TLS 1.3)                      │
│  • Sensitive data in env vars (never in code)           │
│  • Database backups encrypted                           │
│  • No PII stored (only business data)                   │
└─────────────────────────────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│              LAYER 5: APPLICATION SECURITY               │
├─────────────────────────────────────────────────────────┤
│  • Input validation (Zod schemas)                       │
│  • SQL injection prevention (parameterized queries)     │
│  • XSS prevention (React auto-escaping + CSP)           │
│  • CSRF protection (SameSite cookies)                   │
│  • File upload validation (type, size, content)         │
└─────────────────────────────────────────────────────────┘
                         ▼
┌─────────────────────────────────────────────────────────┐
│                 LAYER 6: MONITORING                      │
├─────────────────────────────────────────────────────────┤
│  • Audit logs (all critical actions)                    │
│  • Error tracking (Sentry)                              │
│  • Failed login alerts                                  │
│  • Unusual activity detection                           │
│  • GDPR compliance monitoring                           │
└─────────────────────────────────────────────────────────┘

OWASP Top 10 Mitigation
ThreatMitigationImplementationA1: Broken Access ControlRLS + JWT validationSupabase RLS policies, auth middlewareA2: Cryptographic FailuresTLS 1.3, AES-256Vercel + Supabase defaultsA3: InjectionParameterized queriesSupabase client libraryA4: Insecure DesignThreat modelingSecurity review before launchA5: Security MisconfigurationSecurity headersNext.js security headers configA6: Vulnerable ComponentsDependency scanningDependabot automated PRsA7: Authentication FailuresStrong passwords + MFASupabase Auth built-inA8: Software/Data IntegrityCode signingVercel deployment verificationA9: Logging FailuresComprehensive loggingAudit log table + SentryA10: SSRFURL validationWhitelist external domains

Security Headers
typescript// next.config.js
const securityHeaders = [
  {
    key: 'X-DNS-Prefetch-Control',
    value: 'on'
  },
  {
    key: 'Strict-Transport-Security',
    value: 'max-age=63072000; includeSubDomains; preload'
  },
  {
    key: 'X-Frame-Options',
    value: 'SAMEORIGIN'
  },
  {
    key: 'X-Content-Type-Options',
    value: 'nosniff'
  },
  {
    key: 'X-XSS-Protection',
    value: '1; mode=block'
  },
  {
    key: 'Referrer-Policy',
    value: 'strict-origin-when-cross-origin'
  },
  {
    key: 'Permissions-Policy',
    value: 'camera=(), microphone=(), geolocation=()'
  },
  {
    key: 'Content-Security-Policy',
    value: `
      default-src 'self';
      script-src 'self' 'unsafe-eval' 'unsafe-inline' https://js.stripe.com;
      style-src 'self' 'unsafe-inline';
      img-src 'self' data: https:;
      font-src 'self' data:;
      connect-src 'self' https://*.supabase.co https://api.openai.com https://api.stripe.com;
      frame-src https://js.stripe.com;
    `.replace(/\s{2,}/g, ' ').trim()
  }
];

module.exports = {
  async headers() {
    return [
      {
        source: '/:path*',
        headers: securityHeaders,
      },
    ];
  },
};

Data Privacy Compliance
GDPR Implementation:
typescript// lib/gdpr.ts

// Right to Access
export async function exportUserData(userId: string) {
  const agency = await getAgency(userId);
  const clients = await getClients(agency.id);
  const reports = await getReports(agency.id);
  
  return {
    agency: sanitizeAgencyData(agency),
    clients: clients.map(sanitizeClientData),
    reports: reports.map(sanitizeReportData),
    exported_at: new Date().toISOString()
  };
}

// Right to Deletion
export async function deleteUserData(userId: string) {
  const agency = await getAgency(userId);
  
  // Delete in order (respects foreign keys)
  await deleteReports(agency.id);
  await deleteClients(agency.id);
  await deleteAgency(agency.id);
  await deleteAuth(userId);
  
  await logAudit({
    action: 'user_data_deleted',
    user_id: userId,
    timestamp: new Date()
  });
}

// Right to Rectification
export async function updateUserData(userId: string, updates: any) {
  const agency = await getAgency(userId);
  
  await updateAgency(agency.id, updates);
  
  await logAudit({
    action: 'user_data_updated',
    user_id: userId,
    changes: updates
  });
}

PART 4: DIRECTORY STRUCTURE

24. COMPLETE FILE TREE
insightengine/
├── .github/
│   └── workflows/
│       ├── deploy.yml                 # CI/CD pipeline
│       ├── test.yml                   # Automated testing
│       └── security-scan.yml          # Dependency vulnerability scanning
│
├── app/                               # Next.js 14 App Router
│   ├── (auth)/                        # Auth layout group
│   │   ├── login/
│   │   │   └── page.tsx              # Login page
│   │   ├── signup/
│   │   │   └── page.tsx              # Signup page
│   │   └── reset-password/
│   │       └── page.tsx              # Password reset
│   │
│   ├── (dashboard)/                   # Dashboard layout group
│   │   ├── layout.tsx                # Dashboard shell (nav, sidebar)
│   │   ├── page.tsx                  # Dashboard home
│   │   │
│   │   ├── clients/
│   │   │   ├── page.tsx              # Clients list
│   │   │   ├── new/
│   │   │   │   └── page.tsx          # Add new client
│   │   │   └── [id]/
│   │   │       ├── page.tsx          # Client detail
│   │   │       └── edit/
│   │   │           └── page.tsx      # Edit client
│   │   │
│   │   ├── reports/
│   │   │   ├── page.tsx              # Reports list
│   │   │   ├── new/
│   │   │   │   └── page.tsx          # Create new report
│   │   │   └── [id]/
│   │   │       ├── page.tsx          # Report detail
│   │   │       └── edit/
│   │   │           └── page.tsx      # Edit analysis
│   │   │
│   │   └── settings/
│   │       ├── page.tsx              # Account settings
│   │       ├── branding/
│   │       │   └── page.tsx          # Branding settings
│   │       └── billing/
│   │           └── page.tsx          # Billing & subscription
│   │
│   ├── api/                           # API routes
│   │   ├── auth/
│   │   │   ├── signup/
│   │   │   │   └── route.ts
│   │   │   ├── login/
│   │   │   │   └── route.ts
│   │   │   └── logout/
│   │   │       └── route.ts
│   │   │
│   │   ├── agencies/
│   │   │   ├── route.ts              # GET, PATCH agency
│   │   │   └── branding/
│   │   │       └── route.ts          # PATCH branding
│   │   │
│   │   ├── clients/
│   │   │   ├── route.ts              # GET, POST clients
│   │   │   └── [id]/
│   │   │       └── route.ts          # GET, PATCH, DELETE client
│   │   │
│   │   ├── reports/
│   │   │   ├── route.ts              # GET reports (list)
│   │   │   ├── upload/
│   │   │   │   └── route.ts          # POST upload CSV
│   │   │   └── [id]/
│   │   │       ├── route.ts          # GET, DELETE report
│   │   │       ├── analyze/
│   │   │       │   └── route.ts      # POST generate analysis
│   │   │       ├── pdf/
│   │   │       │   └── route.ts      # POST generate PDF
│   │   │       └── download/
│   │   │           └── route.ts      # GET download PDF
│   │   │
│   │   ├── usage/
│   │   │   └── route.ts              # GET usage stats
│   │   │
│   │   ├── billing/
│   │   │   ├── checkout/
│   │   │   │   └── route.ts          # POST create checkout session
│   │   │   ├── portal/
│   │   │   │   └── route.ts          # POST customer portal
│   │   │   └── webhook/
│   │   │       └── route.ts          # POST Stripe webhooks
│   │   │
│   │   └── health/
│   │       └── route.ts              # GET health check
│   │
│   ├── layout.tsx                     # Root layout
│   ├── page.tsx                       # Landing page (public)
│   ├── globals.css                    # Global styles
│   └── error.tsx                      # Error boundary
│
├── components/                        # React components
│   ├── ui/                            # shadcn/ui components
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── card.tsx
│   │   ├── dialog.tsx
│   │   ├── dropdown-menu.tsx
│   │   ├── select.tsx
│   │   ├── table.tsx
│   │   ├── toast.tsx
│   │   └── ...
│   │
│   ├── dashboard/
│   │   ├── nav.tsx                   # Dashboard navigation
│   │   ├── sidebar.tsx               # Dashboard sidebar
│   │   └── usage-card.tsx            # Usage tracking widget
│   │
│   ├── reports/
│   │   ├── csv-upload.tsx            # CSV upload component
│   │   ├── validation-error.tsx      # Validation error display
│   │   ├── analysis-preview.tsx      # AI analysis preview
│   │   ├── analysis-editor.tsx       # Edit analysis modal
│   │   ├── report-list.tsx           # Reports table
│   │   └── pdf-preview.tsx           # PDF preview iframe
│   │
│   ├── clients/
│   │   ├── client-form.tsx           # Add/edit client form
│   │   └── client-list.tsx           # Clients table
│   │
│   ├── settings/
│   │   ├── branding-form.tsx         # Logo + color picker
│   │   ├── account-form.tsx          # Account settings
│   │   └── subscription-card.tsx     # Subscription info
│   │
│   └── common/
│       ├── loading-spinner.tsx
│       ├── empty-state.tsx
│       ├── error-message.tsx
│       └── confirmation-dialog.tsx
│
├── lib/                               # Utility libraries
│   ├── supabase/
│   │   ├── client.ts                 # Browser Supabase client
│   │   ├── server.ts                 # Server Supabase client
│   │   ├── middleware.ts             # Auth middleware
│   │   └── types.ts                  # Database types (generated)
│   │
│   ├── ai/
│   │   ├── openai.ts                 # OpenAI client wrapper
│   │   ├── prompts.ts                # AI prompt templates
│   │   └── analysis.ts               # Analysis processing logic
│   │
│   ├── pdf/
│   │   ├── generator.ts              # PDF generation logic
│   │   ├── templates.ts              # PDF templates
│   │   └── styles.ts                 # PDF styling
│   │
│   ├── csv/
│   │   ├── parser.ts                 # CSV parsing logic
│   │   ├── validator.ts              # CSV validation rules
│   │   └── schema.ts                 # Expected CSV schema
│   │
│   ├── stripe/
│   │   ├── client.ts                 # Stripe client
│   │   ├── checkout.ts               # Checkout session logic
│   │   ├── webhooks.ts               # Webhook handlers
│   │   └── subscriptions.ts          # Subscription management
│   │
│   ├── email/
│   │   ├── client.ts                 # Postmark client
│   │   ├── templates.ts              # Email templates
│   │   └── send.ts                   # Email sending logic
│   │
│   ├── validations/
│   │   ├── auth.ts                   # Auth input validation (Zod)
│   │   ├── reports.ts                # Report input validation
│   │   ├── clients.ts                # Client input validation
│   │   └── settings.ts               # Settings validation
│   │
│   └── utils/
│       ├── auth.ts                   # Auth helpers
│       ├── errors.ts                 # Error handling
│       ├── rate-limit.ts             # Rate limiting logic
│       └── helpers.ts                # General utilities
│
├── hooks/                             # Custom React hooks
│   ├── use-auth.ts                   # Authentication hook
│   ├── use-toast.ts                  # Toast notifications hook
│   ├── use-reports.ts                # Reports data fetching
│   ├── use-clients.ts                # Clients data fetching
│   └── use-usage.ts                  # Usage tracking hook
│
├── types/                             # TypeScript type definitions
│   ├── database.ts                   # Database types
│   ├── api.ts                        # API request/response types
│   ├── reports.ts                    # Report-related types
│   └── index.ts                      # Exported types
│
├── config/                            # Configuration files
│   ├── site.ts                       # Site metadata
│   ├── plans.ts                      # Subscription plans config
│   └── limits.ts                     # Usage limits config
│
├── middleware.ts                      # Next.js middleware (auth)
│
├── public/                            # Static assets
│   ├── images/
│   │   ├── logo.svg
│   │   ├── hero.png
│   │   └── empty-states/
│   │       ├── no-clients.svg
│   │       └── no-reports.svg
│   ├── favicon.ico
│   └── robots.txt
│
├── docs/                              # Documentation
│   ├── API.md                        # API documentation
│   ├── DEPLOYMENT.md                 # Deployment guide
│├── docs/                              # Documentation
│   ├── API.md                        # API documentation
│   ├── DEPLOYMENT.md                 # Deployment guide
│   ├── SECURITY.md                   # Security practices
│   ├── CONTRIBUTING.md               # Contribution guidelines
│   └── CHANGELOG.md                  # Version history
│
├── tests/                             # Test files
│   ├── unit/
│   │   ├── csv-parser.test.ts
│   │   ├── csv-validator.test.ts
│   │   ├── ai-analysis.test.ts
│   │   └── pdf-generator.test.ts
│   │
│   ├── integration/
│   │   ├── api/
│   │   │   ├── reports.test.ts
│   │   │   ├── clients.test.ts
│   │   │   └── auth.test.ts
│   │   └── workflows/
│   │       └── report-generation.test.ts
│   │
│   ├── e2e/
│   │   ├── signup-flow.spec.ts
│   │   ├── report-creation.spec.ts
│   │   └── billing-flow.spec.ts
│   │
│   └── fixtures/
│       ├── sample-csvs/
│       │   ├── valid-meta-ads.csv
│       │   ├── invalid-missing-columns.csv
│       │   └── invalid-bad-data.csv
│       └── mock-responses/
│           ├── openai-analysis.json
│           └── stripe-webhook.json
│
├── scripts/                           # Utility scripts
│   ├── seed-database.ts              # Seed dev database
│   ├── migrate-database.ts           # Run migrations
│   ├── reset-monthly-usage.ts        # Manual usage reset
│   ├── generate-types.ts             # Generate DB types from Supabase
│   └── test-ai-prompts.ts            # Test AI prompt variations
│
├── supabase/                          # Supabase configuration
│   ├── migrations/
│   │   ├── 20240101000000_initial_schema.sql
│   │   ├── 20240102000000_add_audit_log.sql
│   │   └── 20240103000000_add_rls_policies.sql
│   │
│   ├── functions/                     # Supabase Edge Functions (future)
│   │   └── .gitkeep
│   │
│   └── config.toml                    # Supabase project config
│
├── .env.local.example                 # Example env variables
├── .env.production.example            # Example production env
├── .eslintrc.json                     # ESLint configuration
├── .gitignore                         # Git ignore rules
├── .prettierrc                        # Prettier configuration
├── next.config.js                     # Next.js configuration
├── package.json                       # NPM dependencies
├── package-lock.json                  # NPM lock file
├── postcss.config.js                  # PostCSS configuration
├── tailwind.config.ts                 # Tailwind CSS configuration
├── tsconfig.json                      # TypeScript configuration
├── jest.config.js                     # Jest configuration
├── playwright.config.ts               # Playwright configuration
├── README.md                          # Project README
└── LICENSE                            # License file (MIT)

25. DIRECTORY ORGANIZATION PRINCIPLES
Principle 1: Feature-Based Organization
Dashboard routes are grouped by feature:
app/(dashboard)/
├── clients/          # Everything related to client management
├── reports/          # Everything related to report generation
└── settings/         # Everything related to account settings
Why: Co-located code is easier to find and maintain. When working on "reports," all related pages are in one place.

Principle 2: Shared Components in /components
UI components hierarchy:
components/
├── ui/               # Primitive, reusable UI components (buttons, inputs)
├── dashboard/        # Dashboard-specific composed components
├── reports/          # Report feature components
├── clients/          # Client feature components
└── common/           # Shared across multiple features
Why: Clear separation between primitive UI (shadcn) and feature-specific composed components.

Principle 3: Business Logic in /lib
No business logic in components:
// ❌ BAD: Logic in component
function UploadCSV() {
  const handleUpload = async (file) => {
    const text = await file.text();
    const parsed = Papa.parse(text);
    // ... validation logic here ...
  };
}

// ✅ GOOD: Logic in lib
import { parseAndValidateCSV } from '@/lib/csv/parser';

function UploadCSV() {
  const handleUpload = async (file) => {
    const result = await parseAndValidateCSV(file);
    if (result.valid) {
      // ...
    }
  };
}
Why: Testable, reusable, maintainable. Components focus on UI, lib handles business logic.

Principle 4: Type Safety Everywhere
All types explicitly defined:
typescript// types/reports.ts
export interface Report {
  id: string;
  agency_id: string;
  client_id: string;
  csv_filename: string;
  parsed_data: CampaignData[];
  ai_analysis: AIAnalysis | null;
  pdf_url: string | null;
  status: ReportStatus;
  created_at: string;
}

export type ReportStatus = 'draft' | 'processing' | 'completed' | 'failed';

export interface CampaignData {
  'Campaign Name': string;
  'Impressions': number;
  'Clicks': number;
  'Spend': number;
  'Conversions': number;
  'CPC': number;
  'CTR': number;
  'CPA': number | null;
}
```

**Why:** Catch errors at compile time, better IDE autocomplete, self-documenting code.

---

### **Principle 5: API Routes Mirror Database Structure**

**API structure matches data model:**
```
/api
├── /agencies        → agencies table
├── /clients         → clients table
├── /reports         → reports table
└── /usage           → derived from agencies.reports_used
```

**Why:** Predictable, RESTful, easy to understand relationship between API and data.

---

### **Principle 6: Separation of Concerns in `/lib`**

**Each subdirectory has single responsibility:**
```
lib/
├── supabase/        # Database access only
├── ai/              # AI/ML logic only
├── pdf/             # PDF generation only
├── csv/             # CSV processing only
├── stripe/          # Payment logic only
└── email/           # Email sending only
```

**Why:** Easy to test, swap implementations, and reason about dependencies.

---

### **Principle 7: Tests Mirror Source Structure**

**Test files match source files:**
```
lib/csv/validator.ts        →  tests/unit/csv-validator.test.ts
app/api/reports/route.ts    →  tests/integration/api/reports.test.ts
Why: Easy to find tests for any given file, ensures comprehensive coverage.

26. KEY FILE DESCRIPTIONS
Core Application Files

app/layout.tsx (Root Layout)
typescript// app/layout.tsx
import { Inter } from 'next/font/google';
import { Toaster } from '@/components/ui/toaster';
import { Providers } from '@/components/providers';
import '@/app/globals.css';

const inter = Inter({ subsets: ['latin'] });

export const metadata = {
  title: 'InsightEngine - AI-Powered Meta Ads Reports',
  description: 'Transform Meta Ads data into branded PDF reports in 60 seconds',
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={inter.className}>
        <Providers>
          {children}
          <Toaster />
        </Providers>
      </body>
    </html>
  );
}
Purpose: Root layout that wraps entire application with global providers, fonts, and styles.

middleware.ts (Authentication Middleware)
typescript// middleware.ts
import { createMiddlewareClient } from '@supabase/auth-helpers-nextjs';
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export async function middleware(req: NextRequest) {
  const res = NextResponse.next();
  const supabase = createMiddlewareClient({ req, res });

  // Refresh session if expired
  const {
    data: { session },
  } = await supabase.auth.getSession();

  // Protected routes require authentication
  const protectedPaths = ['/dashboard', '/clients', '/reports', '/settings'];
  const isProtectedPath = protectedPaths.some(path =>
    req.nextUrl.pathname.startsWith(path)
  );

  if (isProtectedPath && !session) {
    // Redirect to login with return URL
    const redirectUrl = req.nextUrl.clone();
    redirectUrl.pathname = '/login';
    redirectUrl.searchParams.set('redirect', req.nextUrl.pathname);
    return NextResponse.redirect(redirectUrl);
  }

  // Public routes redirect authenticated users to dashboard
  const publicPaths = ['/login', '/signup'];
  const isPublicPath = publicPaths.some(path =>
    req.nextUrl.pathname.startsWith(path)
  );

  if (isPublicPath && session) {
    return NextResponse.redirect(new URL('/dashboard', req.url));
  }

  return res;
}

export const config = {
  matcher: [
    '/((?!_next/static|_next/image|favicon.ico|.*\\.(?:svg|png|jpg|jpeg|gif|webp)$).*)',
  ],
};
Purpose: Handles authentication checks, protects routes, manages redirects.

lib/supabase/server.ts (Server-Side Supabase Client)
typescript// lib/supabase/server.ts
import { createServerClient } from '@supabase/ssr';
import { cookies } from 'next/headers';

export function createClient() {
  const cookieStore = cookies();

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return cookieStore.get(name)?.value;
        },
        set(name: string, value: string, options: CookieOptions) {
          cookieStore.set({ name, value, ...options });
        },
        remove(name: string, options: CookieOptions) {
          cookieStore.set({ name, value: '', ...options });
        },
      },
    }
  );
}

// Helper to get current user
export async function getCurrentUser() {
  const supabase = createClient();
  const {
    data: { user },
  } = await supabase.auth.getUser();
  return user;
}

// Helper to get current agency
export async function getCurrentAgency() {
  const supabase = createClient();
  const user = await getCurrentUser();

  if (!user) return null;

  const { data: agency } = await supabase
    .from('agencies')
    .select('*')
    .eq('user_id', user.id)
    .single();

  return agency;
}
Purpose: Server-side Supabase client with cookie-based authentication. Used in Server Components and API routes.

lib/csv/validator.ts (CSV Validation Logic)
typescript// lib/csv/validator.ts
import Papa from 'papaparse';

const REQUIRED_COLUMNS = [
  'Campaign Name',
  'Impressions',
  'Clicks',
  'Spend',
  'Conversions',
  'CPC',
  'CTR',
  'CPA',
];

export interface ValidationResult {
  valid: boolean;
  data?: any[];
  errors?: ValidationError[];
  warnings?: string[];
  metadata?: {
    rows: number;
    campaigns: number;
    totalSpend: number;
    totalConversions: number;
  };
}

export interface ValidationError {
  type: 'MISSING_COLUMNS' | 'INVALID_DATA' | 'PARSE_ERROR';
  message: string;
  details?: any;
}

export async function validateCSV(file: File): Promise<ValidationResult> {
  // Step 1: Parse CSV
  const text = await file.text();
  const parseResult = Papa.parse(text, {
    header: true,
    skipEmptyLines: true,
    dynamicTyping: true,
  });

  if (parseResult.errors.length > 0) {
    return {
      valid: false,
      errors: [
        {
          type: 'PARSE_ERROR',
          message: 'Failed to parse CSV file',
          details: parseResult.errors,
        },
      ],
    };
  }

  // Step 2: Validate columns
  const headers = Object.keys(parseResult.data[0] || {});
  const missingColumns = REQUIRED_COLUMNS.filter(col => !headers.includes(col));

  if (missingColumns.length > 0) {
    return {
      valid: false,
      errors: [
        {
          type: 'MISSING_COLUMNS',
          message: 'Missing required columns',
          details: {
            missing: missingColumns,
            found: headers,
          },
        },
      ],
    };
  }

  // Step 3: Validate data types and ranges
  const validationErrors: ValidationError[] = [];
  const warnings: string[] = [];

  parseResult.data.forEach((row: any, index: number) => {
    // Validate Impressions
    if (!Number.isInteger(row.Impressions) || row.Impressions < 0) {
      validationErrors.push({
        type: 'INVALID_DATA',
        message: `Row ${index + 2}: Impressions must be a non-negative integer`,
        details: { row: index + 2, field: 'Impressions', value: row.Impressions },
      });
    }

    // Validate Clicks
    if (!Number.isInteger(row.Clicks) || row.Clicks < 0) {
      validationErrors.push({
        type: 'INVALID_DATA',
        message: `Row ${index + 2}: Clicks must be a non-negative integer`,
        details: { row: index + 2, field: 'Clicks', value: row.Clicks },
      });
    }

    // Validate Clicks <= Impressions
    if (row.Clicks > row.Impressions) {
      validationErrors.push({
        type: 'INVALID_DATA',
        message: `Row ${index + 2}: Clicks cannot exceed Impressions`,
        details: { row: index + 2, clicks: row.Clicks, impressions: row.Impressions },
      });
    }

    // Validate Spend
    if (typeof row.Spend !== 'number' || row.Spend < 0) {
      validationErrors.push({
        type: 'INVALID_DATA',
        message: `Row ${index + 2}: Spend must be a non-negative number`,
        details: { row: index + 2, field: 'Spend', value: row.Spend },
      });
    }

    // Validate CTR range
    if (row.CTR < 0 || row.CTR > 100) {
      validationErrors.push({
        type: 'INVALID_DATA',
        message: `Row ${index + 2}: CTR must be between 0 and 100`,
        details: { row: index + 2, field: 'CTR', value: row.CTR },
      });
    }

    // Warning for zero spend
    if (row.Spend === 0) {
      warnings.push(`Row ${index + 2}: Campaign "${row['Campaign Name']}" has zero spend`);
    }
  });

  if (validationErrors.length > 0) {
    return {
      valid: false,
      errors: validationErrors,
    };
  }

  // Step 4: Calculate metadata
  const totalSpend = parseResult.data.reduce((sum: number, row: any) => sum + row.Spend, 0);
  const totalConversions = parseResult.data.reduce(
    (sum: number, row: any) => sum + row.Conversions,
    0
  );

  return {
    valid: true,
    data: parseResult.data,
    warnings: warnings.length > 0 ? warnings : undefined,
    metadata: {
      rows: parseResult.data.length,
      campaigns: parseResult.data.length,
      totalSpend: Math.round(totalSpend * 100) / 100,
      totalConversions,
    },
  };
}
Purpose: Core CSV validation logic. Used by upload API route. Ensures data quality before processing.

lib/ai/analysis.ts (AI Analysis Logic)
typescript// lib/ai/analysis.ts
import OpenAI from 'openai';
import type { CampaignData } from '@/types/reports';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

export interface AIAnalysis {
  summary: string;
  wins: string[];
  concerns: string[];
  recommendations: string[];
}

export async function generateAnalysis(
  campaigns: CampaignData[]
): Promise<AIAnalysis> {
  // Calculate aggregate metrics
  const totalImpressions = campaigns.reduce((sum, c) => sum + c.Impressions, 0);
  const totalClicks = campaigns.reduce((sum, c) => sum + c.Clicks, 0);
  const totalSpend = campaigns.reduce((sum, c) => sum + c.Spend, 0);
  const totalConversions = campaigns.reduce((sum, c) => sum + c.Conversions, 0);
  const avgCTR = (totalClicks / totalImpressions) * 100;
  const avgCPA = totalSpend / totalConversions;

  // Construct prompt
  const systemPrompt = `You are an expert Meta Ads analyst with 10+ years of experience. 
Analyze campaign performance data and provide actionable insights.

CRITICAL: Respond ONLY with valid JSON matching this exact structure:
{
  "summary": "2-3 sentence executive overview with specific metrics",
  "wins": [
    "Campaign-specific win with numbers",
    "Campaign-specific win with numbers",
    "Campaign-specific win with numbers"
  ],
  "concerns": [
    "Specific issue with campaign name and metric",
    "Specific issue with campaign name and metric"
  ],
  "recommendations": [
    "Actionable step with campaign name",
    "Actionable step with campaign name",
    "Actionable step with campaign name"
  ]
}

Rules:
- Use specific campaign names from the data
- Include actual numbers from the data
- Be direct and actionable (e.g., "Increase budget by $X")
- No generic statements
- CTR benchmarks: >2.5% excellent, 1.5-2.5% good, <1.5% needs improvement
- Flag campaigns with 0 conversions despite significant spend`;

  const userPrompt = `Analyze this Meta Ads performance data:

Aggregate Metrics:
- Total Spend: $${totalSpend.toFixed(2)}
- Total Impressions: ${totalImpressions.toLocaleString()}
- Total Clicks: ${totalClicks.toLocaleString()}
- Total Conversions: ${totalConversions}
- Overall CTR: ${avgCTR.toFixed(2)}%
- Overall CPA: $${avgCPA.toFixed(2)}

Campaign Details:
${JSON.stringify(campaigns, null, 2)}

Provide analysis in the required JSON format.`;

  try {
    const completion = await openai.chat.completions.create({
      model: 'gpt-4o-mini',
      messages: [
        { role: 'system', content: systemPrompt },
        { role: 'user', content: userPrompt },
      ],
      response_format: { type: 'json_object' },
      temperature: 0.3,
      max_tokens: 1500,
    });

    const content = completion.choices[0].message.content;
    if (!content) {
      throw new Error('Empty response from OpenAI');
    }

    const analysis: AIAnalysis = JSON.parse(content);

    // Validate structure
    if (
      !analysis.summary ||
      !Array.isArray(analysis.wins) ||
      analysis.wins.length !== 3 ||
      !Array.isArray(analysis.concerns) ||
      analysis.concerns.length !== 2 ||
      !Array.isArray(analysis.recommendations) ||
      analysis.recommendations.length !== 3
    ) {
      throw new Error('Invalid response structure from OpenAI');
    }

    return analysis;
  } catch (error) {
    console.error('AI analysis error:', error);
    throw new Error('Failed to generate AI analysis');
  }
}

// Retry wrapper
export async function generateAnalysisWithRetry(
  campaigns: CampaignData[],
  maxRetries = 2
): Promise<AIAnalysis> {
  let lastError: Error | null = null;

  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await generateAnalysis(campaigns);
    } catch (error) {
      lastError = error as Error;
      console.log(`Analysis attempt ${attempt} failed, retrying...`);

      if (attempt < maxRetries) {
        // Wait 2 seconds before retry
        await new Promise(resolve => setTimeout(resolve, 2000));
      }
    }
  }

  throw lastError || new Error('Failed to generate analysis after retries');
}
Purpose: OpenAI integration for generating AI-powered insights. Includes retry logic and validation.

lib/pdf/generator.ts (PDF Generation Logic)
typescript// lib/pdf/generator.ts
import { Document, Page, Text, View, Image, pdf, StyleSheet } from '@react-pdf/renderer';
import type { AIAnalysis } from '@/lib/ai/analysis';

interface GeneratePDFOptions {
  clientName: string;
  reportDate: string;
  analysis: AIAnalysis;
  branding: {
    logoUrl?: string;
    brandColor: string;
    agencyName: string;
  };
}

// PDF Styles
const styles = StyleSheet.create({
  page: {
    padding: 40,
    fontFamily: 'Helvetica',
    fontSize: 11,
  },
  header: {
    flexDirection: 'row',
    marginBottom: 30,
    borderBottom: '2pt solid #e5e7eb',
    paddingBottom: 20,
  },
  logo: {
    width: 60,
    height: 60,
  },
  headerText: {
    marginLeft: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#111827',
  },
  subtitle: {
    fontSize: 12,
    color: '#6b7280',
    marginTop: 4,
  },
  section: {
    marginBottom: 20,
  },
  sectionHeader: {
    fontSize: 16,
    fontWeight: 'bold',
    marginBottom: 10,
    color: '#111827',
  },
  summaryBox: {
    backgroundColor: '#f9fafb',
    padding: 12,
    borderRadius: 8,
  },
  bulletItem: {
    flexDirection: 'row',
    marginBottom: 8,
    marginLeft: 10,
  },
  bulletIcon: {
    width: 20,
    height: 20,
    borderRadius: '50%',
    justifyContent: 'center',
    alignItems: 'center',
    marginRight: 10,
  },
  greenCheck: {
    backgroundColor: '#10b981',
    color: '#ffffff',
  },
  amberWarning: {
    backgroundColor: '#f59e0b',
    color: '#ffffff',
  },
  numberCircle: {
    width: 24,
    height: 24,
    borderRadius: '50%',
    justifyContent: 'center',
    alignItems: 'center',
    marginRight: 10,
  },
  bulletText: {
    flex: 1,
    fontSize: 10,
    lineHeight: 1.6,
    color: '#4b5563',
  },
  footer: {
    position: 'absolute',
    bottom: 40,
    left: 40,
    right: 40,
    textAlign: 'center',
    fontSize: 8,
    color: '#9ca3af',
  },
});

export async function generatePDF(options: GeneratePDFOptions): Promise<Buffer> {
  const { clientName, reportDate, analysis, branding } = options;

  const doc = (
    <Document>
      <Page size="A4" style={styles.page}>
        {/* Header */}
        <View style={styles.header}>
          {branding.logoUrl && (
            <Image src={branding.logoUrl} style={styles.logo} />
          )}
          <View style={styles.headerText}>
            <Text style={styles.title}>Meta Ads Performance Report</Text>
            <Text style={styles.subtitle}>{clientName} • {reportDate}</Text>
          </View>
        </View>

        {/* Executive Summary */}
        <View style={styles.section}>
          <Text style={styles.sectionHeader}>Executive Summary</Text>
          <View style={styles.summaryBox}>
            <Text>{analysis.summary}</Text>
          </View>
        </View>

        {/* Key Wins */}
        <View style={styles.section}>
          <Text style={[styles.sectionHeader, { color: branding.brandColor }]}>
            ✓ Key Wins
          </Text>
          {analysis.wins.map((win, index) => (
            <View key={index} style={styles.bulletItem}>
              <View style={[styles.bulletIcon, styles.greenCheck]}>
                <Text style={{ fontSize: 12, color: '#ffffff' }}>✓</Text>
              </View>
              <Text style={styles.bulletText}>{win}</Text>
            </View>
          ))}
        </View>

        {/* Areas of Concern */}
        <View style={styles.section}>
          <Text style={[styles.sectionHeader, { color: branding.brandColor }]}>
            ⚠ Areas of Concern
          </Text>
          {analysis.concerns.map((concern, index) => (
            <View key={index} style={styles.bulletItem}>
              <View style={[styles.bulletIcon, styles.amberWarning]}>
                <Text style={{ fontSize: 12, color: '#ffffff' }}>⚠</Text>
              </View>
              <Text style={styles.bulletText}>{concern}</Text>
            </View>
          ))}
        </View>

        {/* Recommendations */}
        <View style={styles.section}>
          <Text style={[styles.sectionHeader, { color: branding.brandColor }]}>
            → Recommended Actions
          </Text>
          {analysis.recommendations.map((rec, index) => (
            <View key={index} style={styles.bulletItem}>
              <View
                style={[
                  styles.numberCircle,
                  { backgroundColor: branding.brandColor },
                ]}
              >
                <Text style={{ fontSize: 12, color: '#ffffff' }}>
                  {index + 1}
                </Text>
              </View>
              <Text style={styles.bulletText}>{rec}</Text>
            </View>
          ))}
        </View>

        {/* Footer */}
        <View style={styles.footer}>
          <Text>Generated by {branding.agencyName} using InsightEngine</Text>
        </View>
      </Page>
    </Document>
  );

  // Generate PDF buffer
  const pdfBuffer = await pdf(doc).toBuffer();
  return pdfBuffer;
}
Purpose: PDF generation using react-pdf. Creates branded, professional reports.

app/api/reports/upload/route.ts (Upload API Route)
typescript// app/api/reports/upload/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { createClient, getCurrentAgency } from '@/lib/supabase/server';
import { validateCSV } from '@/lib/csv/validator';

export async function POST(req: NextRequest) {
  try {
    // Get current user and agency
    const agency = await getCurrentAgency();
    if (!agency) {
      return NextResponse.json(
        { success: false, error: 'Unauthorized' },
        { status: 401 }
      );
    }

    // Parse form data
    const formData = await req.formData();
    const file = formData.get('file') as File;
    const clientId = formData.get('client_id') as string;

    if (!file || !clientId) {
      return NextResponse.json(
        { success: false, error: 'Missing file or client_id' },
        { status: 400 }
      );
    }

    // Validate file size (10MB max)
    const maxSize = 10 * 1024 * 1024; // 10MB
    if (file.size > maxSize) {
      return NextResponse.json(
        {
          success: false,
          error: {
            code: 'FILE_TOO_LARGE',
            message: 'File size exceeds 10MB limit',
            details: { size: file.size, limit: maxSize },
          },
        },
        { status: 413 }
      );
    }

    // Validate CSV
    const validationResult = await validateCSV(file);

    if (!validationResult.valid) {
      return NextResponse.json(
        {
          success: false,
          error: {
            code: validationResult.errors![0].type,
            message: validationResult.errors![0].message,
            details: validationResult.errors![0].details,
          },
        },
        { status: 400 }
      );
    }

    // Store CSV in Supabase Storage
    const supabase = createClient();
    const fileName = `${agency.id}/${Date.now()}-${file.name}`;
    const { error: uploadError } = await supabase.storage
      .from('csv-uploads')
      .upload(fileName, file);

    if (uploadError) {
      console.error('Storage upload error:', uploadError);
      return NextResponse.json(
        { success: false, error: 'Failed to upload file' },
        { status: 500 }
      );
    }

    // Create report record
    const { data: report, error: dbError } = await supabase
      .from('reports')
      .insert({
        agency_id: agency.id,
        client_id: clientId,
        csv_filename: file.name,
        csv_storage_path: fileName,
        parsed_data: validationResult.data,
        status: 'draft',
        metadata: validationResult.metadata,
      })
      .select('*, clients(client_name)')
      .single();

    if (dbError) {
      console.error('Database error:', dbError);
      return NextResponse.json(
        { success: false, error: 'Failed to create report' },
        { status: 500 }
      );
    }

    // Return success
    return NextResponse.json(
      {
        success: true,
        data: {
          report_id: report.id,
          client_name: report.clients.client_name,
          validation: validationResult.metadata
           validation: validationResult.metadata,
          warnings: validationResult.warnings,
          status: report.status,
          created_at: report.created_at,
        },
      },
      { status: 201 }
    );
  } catch (error) {
    console.error('Upload error:', error);
    return NextResponse.json(
      { success: false, error: 'Internal server error' },
      { status: 500 }
    );
  }
}
Purpose: Handles CSV file uploads, validates data, stores in database. Entry point for report generation workflow.

app/api/reports/[id]/analyze/route.ts (Analysis API Route)
typescript// app/api/reports/[id]/analyze/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { createClient, getCurrentAgency } from '@/lib/supabase/server';
import { generateAnalysisWithRetry } from '@/lib/ai/analysis';

export async function POST(
  req: NextRequest,
  { params }: { params: { id: string } }
) {
  try {
    const reportId = params.id;
    const agency = await getCurrentAgency();

    if (!agency) {
      return NextResponse.json(
        { success: false, error: 'Unauthorized' },
        { status: 401 }
      );
    }

    const supabase = createClient();

    // Fetch report
    const { data: report, error: fetchError } = await supabase
      .from('reports')
      .select('*')
      .eq('id', reportId)
      .eq('agency_id', agency.id)
      .single();

    if (fetchError || !report) {
      return NextResponse.json(
        { success: false, error: 'Report not found' },
        { status: 404 }
      );
    }

    // Check if already processing (idempotency)
    if (report.status === 'processing') {
      return NextResponse.json(
        {
          success: false,
          error: {
            code: 'ALREADY_PROCESSING',
            message: 'Report is already being analyzed',
            details: {
              report_id: reportId,
              current_status: report.status,
              started_at: report.processing_started_at,
            },
          },
        },
        { status: 409 }
      );
    }

    // Set status to processing
    const { error: lockError } = await supabase
      .from('reports')
      .update({
        status: 'processing',
        processing_started_at: new Date().toISOString(),
      })
      .eq('id', reportId);

    if (lockError) {
      console.error('Failed to lock report:', lockError);
      return NextResponse.json(
        { success: false, error: 'Failed to start analysis' },
        { status: 500 }
      );
    }

    // Generate AI analysis (with retry)
    const startTime = Date.now();
    try {
      const analysis = await generateAnalysisWithRetry(report.parsed_data);
      const processingTime = Date.now() - startTime;

      // Store analysis
      const { error: updateError } = await supabase
        .from('reports')
        .update({
          ai_analysis: analysis,
          status: 'completed',
          completed_at: new Date().toISOString(),
        })
        .eq('id', reportId);

      if (updateError) {
        throw new Error('Failed to store analysis');
      }

      return NextResponse.json({
        success: true,
        data: {
          report_id: reportId,
          status: 'completed',
          analysis,
          confidence_score: 0.92, // Static for MVP
          processing_time_ms: processingTime,
          completed_at: new Date().toISOString(),
        },
      });
    } catch (analysisError) {
      // Analysis failed - update status
      await supabase
        .from('reports')
        .update({
          status: 'failed',
          error_message: (analysisError as Error).message,
          retry_count: report.retry_count + 1,
        })
        .eq('id', reportId);

      console.error('Analysis error:', analysisError);
      return NextResponse.json(
        {
          success: false,
          error: {
            code: 'ANALYSIS_FAILED',
            message: 'Failed to generate AI analysis',
            details: 'Our team has been notified. Please try again in a few minutes.',
          },
        },
        { status: 500 }
      );
    }
  } catch (error) {
    console.error('Analyze endpoint error:', error);
    return NextResponse.json(
      { success: false, error: 'Internal server error' },
      { status: 500 }
    );
  }
}
Purpose: Generates AI analysis for uploaded report. Includes idempotency checks and retry logic.

app/api/reports/[id]/pdf/route.ts (PDF Generation API Route)
typescript// app/api/reports/[id]/pdf/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { createClient, getCurrentAgency } from '@/lib/supabase/server';
import { generatePDF } from '@/lib/pdf/generator';

export async function POST(
  req: NextRequest,
  { params }: { params: { id: string } }
) {
  try {
    const reportId = params.id;
    const agency = await getCurrentAgency();

    if (!agency) {
      return NextResponse.json(
        { success: false, error: 'Unauthorized' },
        { status: 401 }
      );
    }

    const supabase = createClient();

    // Fetch report with all necessary data
    const { data: report, error: fetchError } = await supabase
      .from('reports')
      .select('*, clients(client_name)')
      .eq('id', reportId)
      .eq('agency_id', agency.id)
      .single();

    if (fetchError || !report) {
      return NextResponse.json(
        { success: false, error: 'Report not found' },
        { status: 404 }
      );
    }

    // Check if analysis is complete
    if (report.status !== 'completed' || !report.ai_analysis) {
      return NextResponse.json(
        {
          success: false,
          error: {
            code: 'ANALYSIS_NOT_COMPLETE',
            message: 'Cannot generate PDF - analysis must complete first',
            current_status: report.status,
          },
        },
        { status: 400 }
      );
    }

    // Generate PDF
    const pdfBuffer = await generatePDF({
      clientName: report.clients.client_name,
      reportDate: new Date(report.created_at).toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
      }),
      analysis: report.ai_analysis,
      branding: {
        logoUrl: agency.logo_url || undefined,
        brandColor: agency.brand_color,
        agencyName: agency.agency_name,
      },
    });

    // Upload PDF to storage
    const fileName = `${reportId}.pdf`;
    const { error: uploadError } = await supabase.storage
      .from('reports')
      .upload(fileName, pdfBuffer, {
        contentType: 'application/pdf',
        upsert: true,
      });

    if (uploadError) {
      console.error('PDF upload error:', uploadError);
      return NextResponse.json(
        { success: false, error: 'Failed to upload PDF' },
        { status: 500 }
      );
    }

    // Get public URL
    const {
      data: { publicUrl },
    } = supabase.storage.from('reports').getPublicUrl(fileName);

    // Update report with PDF URL
    const { error: updateError } = await supabase
      .from('reports')
      .update({ pdf_url: publicUrl })
      .eq('id', reportId);

    if (updateError) {
      console.error('Failed to update PDF URL:', updateError);
    }

    // Increment usage counter
    await supabase.rpc('increment_usage', { agency_id: agency.id });

    return NextResponse.json({
      success: true,
      data: {
        report_id: reportId,
        pdf_url: publicUrl,
        file_size_kb: Math.round(pdfBuffer.length / 1024),
        expires_at: null, // PDFs don't expire in MVP
        download_url: publicUrl,
      },
    });
  } catch (error) {
    console.error('PDF generation error:', error);
    return NextResponse.json(
      { success: false, error: 'Internal server error' },
      { status: 500 }
    );
  }
}
Purpose: Generates PDF from completed AI analysis and uploads to storage.

Component Examples

components/reports/csv-upload.tsx (CSV Upload Component)
typescript// components/reports/csv-upload.tsx
'use client';

import { useState } from 'react';
import { Upload, FileText, AlertCircle } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { Alert, AlertDescription } from '@/components/ui/alert';

interface CSVUploadProps {
  onUploadSuccess: (reportId: string) => void;
  clientId: string;
}

export function CSVUpload({ onUploadSuccess, clientId }: CSVUploadProps) {
  const [isDragging, setIsDragging] = useState(false);
  const [isUploading, setIsUploading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const handleDrop = async (e: React.DragEvent<HTMLDivElement>) => {
    e.preventDefault();
    setIsDragging(false);

    const files = Array.from(e.dataTransfer.files);
    if (files.length === 0) return;

    await handleFile(files[0]);
  };

  const handleFileInput = async (e: React.ChangeEvent<HTMLInputElement>) => {
    const files = Array.from(e.target.files || []);
    if (files.length === 0) return;

    await handleFile(files[0]);
  };

  const handleFile = async (file: File) => {
    setError(null);

    // Validate file type
    if (!file.name.endsWith('.csv')) {
      setError('Please upload a CSV file. Excel files must be saved as CSV first.');
      return;
    }

    // Validate file size (10MB)
    if (file.size > 10 * 1024 * 1024) {
      setError('File size exceeds 10MB limit. Please contact support for enterprise processing.');
      return;
    }

    setIsUploading(true);

    try {
      const formData = new FormData();
      formData.append('file', file);
      formData.append('client_id', clientId);

      const response = await fetch('/api/reports/upload', {
        method: 'POST',
        body: formData,
      });

      const result = await response.json();

      if (!result.success) {
        setError(result.error.message || 'Upload failed');
        return;
      }

      onUploadSuccess(result.data.report_id);
    } catch (err) {
      setError('Failed to upload file. Please try again.');
      console.error('Upload error:', err);
    } finally {
      setIsUploading(false);
    }
  };

  return (
    <div className="space-y-4">
      {/* Upload Zone */}
      <div
        className={`
          relative border-2 border-dashed rounded-lg p-12 text-center
          transition-colors cursor-pointer
          ${isDragging ? 'border-primary bg-primary/5' : 'border-gray-300 hover:border-gray-400'}
          ${isUploading ? 'pointer-events-none opacity-50' : ''}
        `}
        onDragOver={(e) => {
          e.preventDefault();
          setIsDragging(true);
        }}
        onDragLeave={() => setIsDragging(false)}
        onDrop={handleDrop}
      >
        <input
          type="file"
          accept=".csv"
          className="absolute inset-0 opacity-0 cursor-pointer"
          onChange={handleFileInput}
          disabled={isUploading}
        />

        {isUploading ? (
          <div className="space-y-2">
            <div className="animate-spin mx-auto h-10 w-10 text-primary">
              <Upload className="h-10 w-10" />
            </div>
            <p className="text-sm font-medium text-gray-900">
              Uploading and validating...
            </p>
          </div>
        ) : (
          <>
            <Upload className="mx-auto h-10 w-10 text-gray-400 mb-3" />
            <p className="text-sm font-medium text-gray-900">
              Drag & drop CSV file here
            </p>
            <p className="text-xs text-gray-500 mt-1">or click to browse</p>
            <div className="mt-4 text-xs text-gray-500">
              <p>Supported: Meta Ads Manager exports</p>
              <p>Max file size: 10MB</p>
            </div>
          </>
        )}
      </div>

      {/* Error Message */}
      {error && (
        <Alert variant="destructive">
          <AlertCircle className="h-4 w-4" />
          <AlertDescription>{error}</AlertDescription>
        </Alert>
      )}

      {/* Help Link */}
      <div className="text-center">
        <p className="text-sm text-gray-600">
          Need help exporting from Meta Ads?{' '}
          
            href="/docs/meta-ads-export"
            target="_blank"
            className="text-primary hover:underline"
          >
            Watch Tutorial Video
          </a>
        </p>
      </div>
    </div>
  );
}
Purpose: File upload component with drag-and-drop, validation, and error handling.

Configuration Files

config/plans.ts (Subscription Plans Configuration)
typescript// config/plans.ts
export const PLANS = {
  trial: {
    name: 'Trial',
    description: '14-day free trial',
    price: 0,
    priceId: null,
    reportLimit: 10,
    features: [
      '10 report generations',
      'AI-powered insights',
      'Branded PDFs',
      'Email support',
    ],
  },
  starter: {
    name: 'Starter',
    description: 'For small agencies',
    price: 99,
    priceId: process.env.STRIPE_STARTER_PRICE_ID,
    reportLimit: 20,
    features: [
      '20 reports per month',
      'AI-powered insights',
      'Branded PDFs',
      'Unlimited clients',
      'Email support',
    ],
  },
  pro: {
    name: 'Pro',
    description: 'For growing agencies',
    price: 199,
    priceId: process.env.STRIPE_PRO_PRICE_ID,
    reportLimit: 100,
    features: [
      '100 reports per month',
      'AI-powered insights',
      'Branded PDFs',
      'Unlimited clients',
      'Priority email support',
      'Live chat support',
    ],
  },
  enterprise: {
    name: 'Enterprise',
    description: 'Custom solution',
    price: null,
    priceId: null,
    reportLimit: 999999,
    features: [
      'Unlimited reports',
      'AI-powered insights',
      'Branded PDFs',
      'Unlimited clients',
      'Dedicated account manager',
      'Phone support',
      'SLA guarantee',
      'Custom integrations',
    ],
  },
} as const;

export type PlanType = keyof typeof PLANS;
Purpose: Centralized configuration for subscription plans. Easy to update pricing and features.

next.config.js (Next.js Configuration)
javascript// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    domains: [
      'localhost',
      '*.supabase.co', // Allow Supabase Storage images
    ],
  },
  
  // Security headers
  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          {
            key: 'X-DNS-Prefetch-Control',
            value: 'on',
          },
          {
            key: 'Strict-Transport-Security',
            value: 'max-age=63072000; includeSubDomains; preload',
          },
          {
            key: 'X-Frame-Options',
            value: 'SAMEORIGIN',
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
          {
            key: 'X-XSS-Protection',
            value: '1; mode=block',
          },
          {
            key: 'Referrer-Policy',
            value: 'strict-origin-when-cross-origin',
          },
        ],
      },
    ];
  },

  // Environment variables validation
  env: {
    NEXT_PUBLIC_APP_URL: process.env.NEXT_PUBLIC_APP_URL,
    NEXT_PUBLIC_SUPABASE_URL: process.env.NEXT_PUBLIC_SUPABASE_URL,
    NEXT_PUBLIC_SUPABASE_ANON_KEY: process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY,
  },

  // Webpack configuration
  webpack: (config) => {
    // Fix for canvas package (used by some PDF libraries)
    config.resolve.alias.canvas = false;
    return config;
  },
};

module.exports = nextConfig;
Purpose: Next.js configuration including security headers, image domains, and webpack customization.

package.json (Dependencies)
json{
  "name": "insightengine",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "type-check": "tsc --noEmit",
    "test:unit": "jest --testPathPattern=tests/unit",
    "test:integration": "jest --testPathPattern=tests/integration",
    "test:e2e": "playwright test",
    "db:migrate": "node scripts/migrate-database.js",
    "db:seed": "node scripts/seed-database.js",
    "generate:types": "supabase gen types typescript --project-id $PROJECT_ID > types/database.ts"
  },
  "dependencies": {
    "next": "^14.1.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "@supabase/ssr": "^0.1.0",
    "@supabase/supabase-js": "^2.39.0",
    "openai": "^4.24.0",
    "stripe": "^14.10.0",
    "@react-pdf/renderer": "^3.1.15",
    "papaparse": "^5.4.1",
    "zod": "^3.22.4",
    "react-hook-form": "^7.49.3",
    "@hookform/resolvers": "^3.3.4",
    "lucide-react": "^0.303.0",
    "tailwind-merge": "^2.2.0",
    "clsx": "^2.1.0",
    "date-fns": "^3.0.6"
  },
  "devDependencies": {
    "@types/node": "^20.10.6",
    "@types/react": "^18.2.46",
    "@types/react-dom": "^18.2.18",
    "@types/papaparse": "^5.3.14",
    "typescript": "^5.3.3",
    "tailwindcss": "^3.4.0",
    "postcss": "^8.4.32",
    "autoprefixer": "^10.4.16",
    "eslint": "^8.56.0",
    "eslint-config-next": "^14.1.0",
    "prettier": "^3.1.1",
    "jest": "^29.7.0",
    "@testing-library/react": "^14.1.2",
    "@testing-library/jest-dom": "^6.1.5",
    "playwright": "^1.40.1",
    "husky": "^8.0.3",
    "lint-staged": "^15.2.0"
  },
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ]
  }
}
Purpose: Project dependencies and scripts for development, testing, and deployment.

SUMMARY
This comprehensive PRD, MVP specification, system architecture, and directory structure provides everything needed to build InsightEngine:
What's Included:
✅ Complete Product Requirements

Problem statement and market analysis
Target users and personas
Feature requirements (MVP and roadmap)
Success metrics and KPIs

✅ MVP Specification

Scope definition and timeline (60 days)
Prioritized feature list
What's NOT included (deferred features)
Launch criteria

✅ System Architecture

High-level architecture diagrams
Complete technology stack
Database schema with RLS
API endpoint specifications
Infrastructure and deployment plan

✅ Complete Directory Structure

Every file and folder explained
Organization principles
Key file examples with full code
Configuration files

Ready to Build:
Week 1-2: Database setup + Authentication
Week 3-4: CSV upload + Client management
Week 5-6: AI integration + Analysis
Week 7: PDF generation
Week 8: Stripe billing
Week 9: Testing + Beta launch
Total Timeline: 60 days to MVP launch
