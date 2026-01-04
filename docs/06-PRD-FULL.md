# Product Requirements Document (PRD)

**Product Name:** ReportForge
**Tagline:** "AI-powered reports, forged in seconds."
**Version:** 1.0 (MVP)
**Last Updated:** January 2026
**Status:** Approved for Development

---

## TABLE OF CONTENTS

**PART 1: PRODUCT REQUIREMENTS**
1.  Executive Summary
2.  Problem Statement
3.  Target Users & Market
4.  Product Vision & Goals
5.  Success Metrics

**PART 2: FEATURE REQUIREMENTS**
6.  Core Features (MVP)
7.  Post-MVP Features (Roadmap)
8.  User Flows

**PART 3: DESIGN & TECHNICAL**
9.  Design Requirements
10. Non-Functional Requirements
11. Constraints & Assumptions
12. Future Roadmap

---

# PART 1: PRODUCT REQUIREMENTS

## 1. EXECUTIVE SUMMARY

### What We're Building
ReportForge is a B2B SaaS platform that transforms raw Meta Ads performance data into branded, AI-powered executive reports in under 60 seconds. Marketing agencies upload CSV exports from Meta Ads Manager and receive professional PDF reports with actionable insights, wins, concerns, and recommendations.

### Core Value Proposition
*   **Time Savings:** Reduce report creation time from 2-3 hours to 60 seconds.
*   **Consistency:** Every report follows a proven structure with professional formatting.
*   **Insights:** AI identifies patterns human analysts might miss.
*   **Branding:** White-labeled with agency logo and colors.
*   **Scalability:** Generate 20-100 reports/month without hiring additional staff.

### Business Model
*   **Pricing:** $99/month (Starter, 20 reports) or $199/month (Pro, 100 reports).
*   **Target Revenue:** $50K MRR within 12 months.
*   **Target Market:** 5,000+ marketing agencies managing Meta Ads in North America.

### Competitive Advantage
*   **CSV-first approach:** No complex API integration required.
*   **AI-powered insights:** Not just data visualization.
*   **White-label from day one:** Agencies can brand as their own.
*   **No learning curve:** 5 minutes from signup to first report.

---

## 2. PROBLEM STATEMENT

### Current Situation
Marketing agencies managing Meta Ads campaigns for clients spend 2-4 hours per client per month creating performance reports. This includes:
1.  Exporting data from Meta Ads Manager.
2.  Copy-pasting metrics into templates.
3.  Writing executive summaries.
4.  Identifying wins and areas for improvement.
5.  Formatting and branding.
6.  Converting to PDF.

For an agency with 10 clients: **20-40 hours/month on reporting alone.**

### Pain Points

**Pain Point 1: Time-Consuming Manual Work**
*   Agencies bill $100-150/hour for strategic work.
*   Reporting is operational, not strategic.
*   30+ hours/month = $3,000-4,500 in lost billable time.

**Pain Point 2: Inconsistent Quality**
*   Different account managers write different quality reports.
*   Junior staff produce generic, unhelpful summaries.
*   Clients notice inconsistency across months.

**Pain Point 3: Existing Tools Don't Solve This**
*   Data visualization tools (Tableau, Looker): Require ongoing setup, not designed for client reports.
*   Reporting dashboards (Supermetrics, Porter): Auto-update data but don't write insights.
*   General automation (Zapier): Can't write intelligent analysis.

**Pain Point 4: Scaling Problem**
*   Adding new clients = adding reporting burden.
*   Agencies turn down business because they can't handle report workload.
*   Hiring junior staff to write reports = training time + quality issues.

### Why Now?
*   **AI Maturity:** GPT-4 can now write coherent, data-driven analysis.
*   **Agency Saturation:** Agencies face pressure to demonstrate ROI more clearly.
*   **Remote Work:** Agencies need tools that work seamlessly across distributed teams.
*   **Cost Pressure:** Economic uncertainty forces agencies to do more with less.

---

## 3. TARGET USERS & MARKET

### Primary Persona: Sarah, the Account Manager

**Demographics:**
*   Age: 28-35
*   Title: Account Manager, Senior Media Buyer, or Client Success Manager
*   Company: Digital marketing agency (5-50 employees)
*   Location: US/Canada
*   Salary: $60K-$90K

**Responsibilities:**
*   Manage 8-15 client accounts.
*   Run Meta Ads campaigns.
*   Report monthly performance to clients.
*   Recommend optimizations.
*   Retain clients through demonstrated value.

**Goals:**
*   Keep clients happy with clear, professional reports.
*   Save time on operational tasks to focus on strategy.
*   Demonstrate value to justify retainers.
*   Hit client KPIs (CPA, ROAS, conversions).

**Pain Points:**
*   Spends 3-4 hours/week on client reporting.
*   Struggles to articulate the "story" behind the numbers.
*   Clients ask "what should we do next?" and she needs recommendations fast.
*   Boss expects high-quality deliverables but she's overworked.

**Technology Profile:**
*   Comfortable with SaaS tools (Slack, Asana, Google Workspace).
*   Not technical (doesn't code).
*   Uses Meta Ads Manager daily.
*   Prefers simple, intuitive interfaces.

---

### Secondary Persona: Mike, the Agency Owner

**Demographics:**
*   Age: 35-50
*   Title: Founder, CEO, or Managing Partner
*   Company: Boutique agency (5-20 employees)
*   Experience: 10+ years in digital marketing

**Responsibilities:**
*   Grow agency revenue and client base.
*   Manage team of 3-15 people.
*   Oversee all client relationships.
*   Control costs and margins.

**Goals:**
*   Scale agency without proportionally hiring more staff.
*   Standardize deliverables across team.
*   Increase profit margins (reduce hours per client).
*   Differentiate agency from competitors.

**Pain Points:**
*   Can't scale because team spends too much time on operational work.
*   Junior staff produce inconsistent quality reports.
*   Clients churn due to lack of clear ROI demonstration.
*   Margins shrinking due to increased operational overhead.

---

### Market Size & Opportunity

**Total Addressable Market (TAM):**
*   50,000+ digital marketing agencies in North America.
*   Average agency manages Meta Ads for 10+ clients.
*   Estimated market: $300M+ annually.

**Serviceable Addressable Market (SAM):**
*   15,000 agencies actively managing Meta Ads as core service.
*   Target: Agencies with 5-50 employees.
*   Estimated market: $90M annually.

**Serviceable Obtainable Market (SOM):**
*   Target 1% of SAM in Year 1 = 150 agencies.
*   Average contract value: $1,400/year.
*   Year 1 Revenue Target: $210K ARR.

---

## 4. PRODUCT VISION & GOALS

### Vision Statement
> "Empower marketing agencies to deliver world-class client reporting in seconds, not hours, so they can focus on strategy instead of operational busywork."

### Product Principles

**1. Simplicity First**
*   No integrations to configure.
*   No training required.
*   Works in 5 minutes or less.

**2. Human-in-the-Loop**
*   AI assists, humans decide.
*   Always editable before sending to clients.
*   Never auto-send reports (agencies review first).

**3. Quality Over Quantity**
*   One platform (Meta Ads) done excellently > 5 platforms done poorly.
*   One report type (executive summary) done well > 10 mediocre templates.

**4. Agency-Centric**
*   White-labeled by default.
*   Agency's brand, not ours.
*   Built for resellers, not end-clients.

---

### Goals for Year 1

**Product Goals:**
*   ✅ MVP launch: Q1 2026
*   ✅ 100 active agencies: Q2 2026
*   ✅ Google Ads support: Q2 2026
*   ✅ API access: Q3 2026
*   ✅ 500 active agencies: Q4 2026

**Business Goals:**
*   $50K MRR by end of Year 1.
*   80%+ customer retention.
*   NPS score >40.
*   <5% monthly churn.

**User Goals:**
*   Each agency saves 15+ hours/month.
*   90%+ of reports require no editing.
*   4.5+ star average rating on G2/Capterra.

---

## 5. SUCCESS METRICS

### North Star Metric
**Reports generated per month** (indicates product usage and value delivery).
*   Target: 10,000 reports/month by end of Year 1.

### Key Performance Indicators (KPIs)

**Product Metrics:**
| Metric | Target (Month 3) | Target (Month 12) |
| :--- | :--- | :--- |
| Monthly Active Agencies | 50 | 500 |
| Reports Generated/Month | 600 | 10,000 |
| Average Reports per Agency | 12 | 20 |
| Report Generation Success Rate | 95% | 98% |
| AI Analysis Quality Score | 3.5/5 | 4.2/5 |

**Business Metrics:**
| Metric | Target (Month 3) | Target (Month 12) |
| :--- | :--- | :--- |
| MRR | $5K | $50K |
| Customer Acquisition Cost (CAC) | <$200 | <$150 |
| Lifetime Value (LTV) | $1,000 | $2,500 |
| LTV:CAC Ratio | 5:1 | 15:1 |
| Monthly Churn Rate | <10% | <5% |
| Net Revenue Retention | 100% | 120% |

**User Satisfaction Metrics:**
| Metric | Target |
| :--- | :--- |
| Net Promoter Score (NPS) | >40 |
| Customer Satisfaction (CSAT) | >4.5/5 |
| Feature Adoption Rate | >80% |
| Time to First Report | <10 min |
| Support Ticket Volume | <5% |

---

# PART 2: FEATURE REQUIREMENTS

## 6. CORE FEATURES (MVP)

### Feature 1: CSV Upload & Validation
**Description:** Users upload CSV files exported from Meta Ads Manager. System validates file structure and data quality before processing.

**User Story:** As an agency account manager, I want to upload my Meta Ads export CSV so that I can quickly generate a client report without manual data entry.

**Acceptance Criteria:**
*   ✅ User can drag-and-drop CSV file or browse to select.
*   ✅ System validates required columns within 5 seconds.
*   ✅ Clear error messages if columns are missing or data is invalid.
*   ✅ User sees summary of validated data (campaign count, spend, date range).
*   ✅ File size limit: 10MB.
*   ✅ Supported format: CSV only.

**Priority:** P0 (Must-Have for MVP)
**Complexity:** Medium

---

### Feature 2: AI-Powered Performance Analysis
**Description:** AI analyzes campaign data and generates structured insights including executive summary, wins, concerns, and recommendations.

**User Story:** As an agency account manager, I want AI to analyze my campaign data and write insights so that I don't spend 30 minutes manually interpreting the numbers.

**Acceptance Criteria:**
*   ✅ Analysis completes in <30 seconds for typical datasets.
*   ✅ Output includes: **Performance Score** (0-100), Quick Summary (1 sentence), detailed summary (2-3 sentences), 3 wins, 2 concerns, 3 recommendations.
*   ✅ **Performance Score** answers "Is it good or bad?" at a glance (80+: Strong, 50-79: Moderate, <50: Needs Attention).
*   ✅ **Quick Summary** is a scannable, single-sentence overview (e.g., "Spent $2,450, generated 89 conversions at $27 CPA. Strong performance.").
*   ✅ AI mentions specific campaign names (not generic).
*   ✅ AI includes actual metrics from data (not vague statements).
*   ✅ User can regenerate analysis if unsatisfied (up to 3 times).
*   ✅ Analysis is editable before PDF generation.
*   ✅ **Personal Note** field available for user to add human touch (optional, but prompted).

**Priority:** P0 (Must-Have for MVP)
**Complexity:** High

---

### Feature 3: Branded PDF Report Generation
**Description:** Convert AI analysis into professionally designed PDF with agency branding (logo, colors).

**User Story:** As an agency owner, I want reports to include my logo and brand colors so that clients see my agency's professional brand, not a third-party tool.

**Acceptance Criteria:**
*   ✅ PDF includes agency logo in header.
*   ✅ Brand color used for section headers and accents.
*   ✅ Professional typography and spacing.
*   ✅ Generates in <10 seconds.
*   ✅ File size <2MB for easy email attachment.
*   ✅ PDF is print-ready (A4 size, 150 DPI).
*   ✅ Download link valid for 12 months.

**Priority:** P0 (Must-Have for MVP)
**Complexity:** Medium

---

### Feature 4: Client Management
**Description:** Users can add, edit, and organize clients. Reports are associated with specific clients.

**User Story:** As an agency account manager, I want to organize reports by client so that I can quickly find past reports and track which clients need reports this month.

**Acceptance Criteria:**
*   ✅ User can add new client (name required, website/industry optional).
*   ✅ User can edit client details.
*   ✅ User can delete client (with confirmation warning).
*   ✅ Client list shows: name, last report date, total report count.
*   ✅ Clicking client shows all their reports.
*   ✅ When creating report, user selects client from dropdown.

**Priority:** P0 (Must-Have for MVP)
**Complexity:** Low

---

### Feature 5: Report History & Management
**Description:** Users can view all past reports, re-download PDFs, and delete old reports.

**User Story:** As an agency account manager, I want to see all past reports I've generated so that I can compare month-over-month performance or re-send a report to a client.

**Acceptance Criteria:**
*   ✅ List view shows: date, client, status, actions.
*   ✅ Filter by client, date range, status.
*   ✅ Sort by date (newest first by default).
*   ✅ Each report row has: Download PDF, View Analysis, Delete buttons.
*   ✅ Delete requires confirmation.
*   ✅ Pagination: 20 reports per page.

**Priority:** P0 (Must-Have for MVP)
**Complexity:** Low

---

### Feature 6: Agency Branding Settings
**Description:** Users configure their agency branding (logo, color) which appears on all generated reports.

**User Story:** As an agency owner, I want to upload my logo and set my brand color once, and have it automatically applied to all reports.

**Acceptance Criteria:**
*   ✅ User can upload logo (PNG/JPG, max 500KB).
*   ✅ Logo preview shown after upload.
*   ✅ User can set brand color via hex input or color picker.
*   ✅ Color preview shown in real-time.
*   ✅ Settings apply to all future reports immediately.
*   ✅ User can update logo/color anytime.

**Priority:** P0 (Must-Have for MVP)
**Complexity:** Low

---

### Feature 7: Usage Tracking & Limits
**Description:** System tracks how many reports user has generated this month and enforces plan limits.

**User Story:** As a Starter plan user, I want to see how many reports I've used this month so that I know when I'm approaching my limit and need to upgrade or wait for reset.

**Acceptance Criteria:**
*   ✅ Dashboard shows: "X of Y reports used this month".
*   ✅ Progress bar visualization.
*   ✅ Warning when 80% of limit reached.
*   ✅ Hard block when 100% limit reached with upgrade prompt.
*   ✅ Usage resets on 1st of each month automatically.
*   ✅ Upgrade link shown when limit reached.

**Priority:** P0 (Must-Have for MVP)
**Complexity:** Low

---

### Feature 8: Subscription Management (Stripe)
**Description:** Users can sign up, upgrade, downgrade, and cancel subscriptions. Payments processed via Stripe.

**User Story:** As an agency owner, I want to manage my subscription (upgrade, downgrade, cancel) without contacting support so that I have control over my costs.

**Acceptance Criteria:**
*   ✅ Free 14-day trial (no credit card required).
*   ✅ After trial, user chooses Starter ($99/mo) or Pro ($199/mo).
*   ✅ User can upgrade immediately (prorated charge).
*   ✅ User can downgrade (takes effect next billing cycle).
*   ✅ User can cancel anytime (access continues until end of period).
*   ✅ User can update payment method.
*   ✅ Automatic email receipts for all charges.

**Priority:** P0 (Must-Have for MVP)
**Complexity:** Medium

---

## 7. POST-MVP FEATURES (Roadmap)

### Feature 9: Google Ads Support (Q2 2026)
Support CSV uploads from Google Ads Manager, not just Meta Ads.
*   **Priority:** P1 | **Complexity:** Medium | **Effort:** 3 weeks

### Feature 10: Custom Report Templates (Q3 2026)
Agencies can customize which sections appear and add custom text blocks.
*   **Priority:** P1 | **Complexity:** High | **Effort:** 4 weeks

### Feature 11: API Access (Q3 2026)
RESTful API for programmatic report generation.
*   **Priority:** P1 | **Complexity:** High | **Effort:** 6 weeks

### Feature 12: Team Collaboration (Q4 2026)
Multi-user accounts with roles, comments, and task assignment.
*   **Priority:** P2 | **Complexity:** High | **Effort:** 8 weeks

### Feature 13: Historical Trend Analysis (Q4 2026)
Compare current month's performance to previous months.
*   **Priority:** P2 | **Complexity:** High | **Effort:** 4 weeks

---

## 8. USER FLOWS

### User Flow 1: First-Time User Onboarding (Optimized for 5-Minute Time-to-Value)
```
START: User signs up
    ↓
1. Enter email + password → Receive verification email → Click link
    ↓
2. Welcome screen: "Let's create your first report in 60 seconds!"
    ↓
3. Prompt: "Who is this report for?" → Enter client name (name only, 10 seconds)
    ↓
4. Upload CSV file (Drag-drop or browse)
    ↓
5. Validation loading (2-5 seconds) → Success: "12 campaigns validated"
    ↓
6. Click "Generate Analysis" → AI processing (15-30 sec)
    ↓
7. Analysis appears with **Performance Score** badge (e.g., "82 - Strong")
    ↓
8. User reviews Quick Summary → Edits if needed → Adds Personal Note (optional)
    ↓
9. Click "Forge PDF" → PDF rendering (5-10 sec)
    ↓
10. Success! Download PDF → Celebration 🎉
    ↓
11. **POST-SUCCESS PROMPT:** "Make it yours! Add your agency logo and brand color."
    → User can add branding now OR skip and do later in Settings.

TIME TO COMPLETE: 3-5 minutes (branding deferred to reduce friction)
```

**Rationale (from Market Research):**
- "5-minute time-to-value is critical for trial conversion."
- Deferring branding until after first success increases emotional investment.

### User Flow 2: Generating a Regular Report (Returning User)
```
START: User logs in
    ↓
1. Dashboard shows: Reports used (8/20), Recent reports, "New Report" button
    ↓
2. Click "New Report"
    ↓
3. Select existing client OR add new client
    ↓
4. Upload CSV → Validation (2-5 sec)
    ↓
5. Click "Generate Analysis" → AI processing (15-30 sec)
    ↓
6. Review analysis → (Optional) Edit if needed
    ↓
7. Click "Forge PDF" → PDF rendering (5-10 sec)
    ↓
8. Download PDF

TIME TO COMPLETE: 60-90 seconds (excluding editing)
```

### User Flow 3: Handling CSV Validation Error
```
START: User uploads CSV
    ↓
1. Validation runs
    ↓
2. ERROR: "Missing required columns: Conversions, CPA"
    ↓
3. Error modal shows: What's wrong, Which columns are missing, How to fix
    ↓
4. User Options: "Watch Tutorial" | "Try Different File" | "Contact Support"
    ↓
5. User re-exports CSV from Meta Ads Manager with correct columns
    ↓
6. Uploads new CSV → Validation succeeds → Continue to analysis

END: User learns correct export process for future
```

### User Flow 4: Upgrading from Trial to Paid
```
START: User has used 10/10 trial reports
    ↓
1. User tries to create 11th report
    ↓
2. Block screen: "Trial Limit Reached"
    ↓
3. Show plans: Starter ($99/mo) | Pro ($199/mo)
    ↓
4. User selects plan → Redirect to Stripe Checkout
    ↓
5. Stripe processes payment (5-10 sec)
    ↓
6. Redirect back → Success: "Welcome to [Plan Name]!"
    ↓
7. User can now create report

END: User is paying customer
```

---

# PART 3: DESIGN & TECHNICAL

## 9. DESIGN REQUIREMENTS

### Design Principles
1.  **Clarity Over Cleverness:** Interface should be self-explanatory. No user should need a tutorial video.
2.  **Speed Over Features:** Fast, focused workflows. No unnecessary steps.
3.  **Professional, Not Flashy:** Clean, modern aesthetic. Agency-appropriate.
4.  **Progressive Disclosure:** Show basics first, advanced options hidden.

### Visual Design Guidelines

**Color Palette:**
```
Primary:    #4f46e5 (Indigo 600)
Success:    #059669 (Emerald 600)
Warning:    #f59e0b (Amber 500)
Error:      #e11d48 (Rose 600)
Neutral:    #0f172a (Slate 900) for text
            #64748b (Slate 500) for secondary text
            #f8fafc (Slate 50) for backgrounds
```

**Typography:**
```
Font Family: Inter (system fallback: -apple-system, sans-serif)

Headings:
  H1: 30px, Bold
  H2: 24px, Semibold
  H3: 20px, Semibold

Body:
  Large:  16px, Regular
  Normal: 14px, Regular
  Small:  12px, Regular

Line Height: 1.5 for body, 1.2 for headings
```

**Spacing System:**
```
4px, 8px, 12px, 16px, 24px, 32px, 48px, 64px
(Use multiples of 4 for consistent rhythm)

Component padding: 16px default, 24px for cards
Section margins: 32px between major sections
Page margins: 48px on desktop, 24px on mobile
```

**Buttons:**
*   **Primary:** Indigo-600 background, White text, 12px 24px padding, 8px radius.
*   **Secondary:** Transparent background, Slate-300 border, Slate-700 text.
*   **Danger:** Rose-600 background, White text.

**Inputs:**
*   Height: 44px (touch-friendly).
*   Border: 1px solid Slate-300.
*   Focus: Indigo-500 ring, 2px offset.

**Cards:**
*   Background: White. Border: 1px solid Slate-200. Radius: 12px. Padding: 24px. Shadow: sm.

---

## 10. NON-FUNCTIONAL REQUIREMENTS

### Security Requirements
**Authentication & Authorization:**
*   JWT tokens for API authentication.
*   Session timeout after 7 days of inactivity.
*   Password requirements: minimum 8 characters.
*   **Row Level Security (RLS):** Users can only access their own data.

**Data Protection:**
*   All data encrypted at rest (AES-256).
*   All data encrypted in transit (TLS 1.3).
*   GDPR compliant (right to access, delete, export data).
*   CCPA compliant.

**Input Validation:**
*   Server-side validation on all inputs.
*   SQL injection prevention (parameterized queries).
*   XSS prevention (sanitize user inputs).
*   CSRF protection.
*   File upload validation (type, size, content).

---

### Reliability Requirements
*   **Uptime Target:** 99.5%.
*   **Scheduled Maintenance:** Sundays 2-4 AM EST.
*   **Backups:** Automated daily. Retained for 30 days.
*   **Error Handling:** Graceful degradation. Automatic retry logic.

### Maintainability Requirements
*   **Code Quality:** TypeScript, ESLint, Prettier. Unit test coverage >70%.
*   **Documentation:** API docs (OpenAPI), README files, ADRs.
*   **Deployment:** CI/CD pipeline, Feature flags, Blue-green deployment.

### Accessibility Requirements (WCAG 2.1 AA)
*   Keyboard navigation for all interactive elements.
*   Screen reader compatible (semantic HTML, ARIA labels).
*   Color contrast ratio ≥4.5:1.
*   Focus indicators visible.

---

## 11. CONSTRAINTS & ASSUMPTIONS

### Constraints
**Technical:**
*   Must use Supabase for database.
*   Must use OpenAI API (`gpt-4o-mini`).
*   Meta Ads CSV format only in MVP.
*   PDF generation must not exceed 20 seconds.
*   File uploads limited to 10MB.

**Business:**
*   Budget: $5K/month for infrastructure + APIs (Year 1).
*   Timeline: MVP must launch within 60 days.
*   Bootstrapped (no external fundraising in Year 1).

**Resource:**
*   OpenAI API budget: $500/month.
*   Storage budget: $100/month.

### Assumptions
**User:**
*   Users are comfortable with basic SaaS tools.
*   Users know how to export CSV from Meta Ads Manager.
*   Users primarily work on desktop/laptop.

**Market:**
*   Agencies will pay $99-199/month for time savings.
*   Agencies prefer CSV uploads over API integrations.
*   AI quality is "good enough" (human review expected).

**Technical:**
*   OpenAI API will remain stable (99%+ uptime).
*   Supabase will scale to 500 agencies.
*   Vercel can handle traffic spikes.

---

## 12. FUTURE ROADMAP

### Q1 2026 (MVP Launch)
*   ✅ Core features: CSV upload, AI analysis, PDF generation.
*   ✅ Subscription management (Stripe).
*   ✅ Meta Ads support only.
*   ✅ 100 beta users.

### Q2 2026 (Expansion)
*   Google Ads support (CSV upload).
*   Email scheduling (send reports directly to clients).
*   Report templates (2-3 different layouts).
*   API v1.0 (read-only access).
*   Target: 250 active agencies.

### Q3 2026 (Scale)
*   LinkedIn Ads support.
*   Team collaboration (multi-user accounts).
*   Custom branding (remove "Powered by" footer for Pro plan).
*   Zapier integration.
*   Historical trend analysis.
*   Target: 500 active agencies.

### Q4 2026 (Enterprise)
*   TikTok Ads support.
*   White-label reseller program.
*   Enterprise plan (custom pricing, dedicated support).
*   Advanced analytics (benchmark against industry averages).
*   Native integrations (HubSpot, Salesforce).
*   Target: 750 active agencies.

### 2027 Roadmap Ideas (Not Committed)
*   Mobile app (iOS/Android).
*   Scheduled report generation (auto-generate monthly).
*   Client portal (clients can log in and view their reports).
*   Video report summaries (AI-generated).
*   Multilingual support (Spanish, French, German).
