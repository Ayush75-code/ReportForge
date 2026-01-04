PRODUCT REQUIREMENTS DOCUMENT (PRD)

Document Information
Product Name: InsightEngine
Version: 1.0 (MVP)
Document Version: 1.0
Last Updated: January 2026
Owner: Product Team
Status: Approved for Development

TABLE OF CONTENTS
PART 1: PRODUCT REQUIREMENTS DOCUMENT

Executive Summary
Problem Statement
Target Users & Market
Product Vision & Goals
Success Metrics
Feature Requirements (Full Product)
User Flows
Technical Requirements
Design Requirements
Non-Functional Requirements
Constraints & Assumptions
Future Roadmap

PART 2: MVP SPECIFICATION

MVP Scope Definition
MVP Feature Set
MVP Success Criteria
What's NOT in MVP
MVP Timeline & Milestones

PART 3: SYSTEM ARCHITECTURE

High-Level Architecture
Technology Stack
Database Architecture
API Architecture
Infrastructure & Deployment
Security Architecture

PART 4: DIRECTORY STRUCTURE

Complete File Tree
Directory Organization Principles
Key File Descriptions


PART 1: PRODUCT REQUIREMENTS DOCUMENT

1. EXECUTIVE SUMMARY
What We're Building
InsightEngine is a B2B SaaS platform that transforms raw Meta Ads performance data into branded, AI-powered executive reports in under 60 seconds. Marketing agencies upload CSV exports from Meta Ads Manager and receive professional PDF reports with actionable insights, wins, concerns, and recommendations.
Core Value Proposition

Time Savings: Reduce report creation time from 2-3 hours to 60 seconds
Consistency: Every report follows proven structure with professional formatting
Insights: AI identifies patterns human analysts might miss
Branding: White-labeled with agency logo and colors
Scalability: Generate 20-100 reports/month without hiring additional staff

Business Model

Pricing: $99/month (Starter, 20 reports) or $199/month (Pro, 100 reports)
Target Revenue: $50K MRR within 12 months
Target Market: 5,000+ marketing agencies managing Meta Ads in North America

Competitive Advantage

CSV-first approach (no API integration hell)
AI-powered insights (not just data visualization)
White-label from day one (agencies can brand as their own)
No learning curve (5 minutes from signup to first report)


2. PROBLEM STATEMENT
Current Situation
Marketing agencies managing Meta Ads campaigns for clients spend 2-4 hours per client per month creating performance reports. This includes:

Exporting data from Meta Ads Manager
Copy-pasting metrics into templates
Writing executive summaries
Identifying wins and areas for improvement
Formatting and branding
Converting to PDF

For an agency with 10 clients: 20-40 hours/month on reporting alone.
Pain Points
Pain Point 1: Time-Consuming Manual Work

Agencies bill $100-150/hour for strategic work
Reporting is operational, not strategic
30+ hours/month = $3,000-4,500 in lost billable time

Pain Point 2: Inconsistent Quality

Different account managers write different quality reports
Junior staff produce generic, unhelpful summaries
Clients notice inconsistency across months

Pain Point 3: Existing Tools Don't Solve This

Data visualization tools (Tableau, Looker): Require ongoing setup, not designed for client reports
Reporting dashboards (Supermetrics, Porter): Auto-update data but don't write insights
General automation (Zapier): Can't write intelligent analysis

Pain Point 4: Scaling Problem

Adding new clients = adding reporting burden
Agencies turn down business because they can't handle report workload
Hiring junior staff to write reports = training time + quality issues

Why Now?

AI Maturity: GPT-4 can now write coherent, data-driven analysis
Agency Saturation: Marketing agencies facing pressure to demonstrate ROI more clearly
Remote Work: Agencies need tools that work seamlessly across distributed teams
Cost Pressure: Economic uncertainty forcing agencies to do more with less


3. TARGET USERS & MARKET
Primary User Persona: Sarah, Agency Account Manager
Demographics:

Age: 28-35
Title: Account Manager, Senior Media Buyer, or Client Success Manager
Company: Digital marketing agency (5-50 employees)
Location: US/Canada
Salary: $60K-$90K

Responsibilities:

Manage 8-15 client accounts
Run Meta Ads campaigns
Report monthly performance to clients
Recommend optimizations
Retain clients through demonstrated value

Goals:

Keep clients happy with clear, professional reports
Save time on operational tasks to focus on strategy
Demonstrate value to justify retainers
Hit client KPIs (CPA, ROAS, conversions)

Pain Points:

Spends 3-4 hours/week on client reporting
Struggles to articulate "story" behind the numbers
Clients ask "what should we do next?" and she needs recommendations fast
Boss expects high-quality deliverables but she's overworked

Technology Profile:

Comfortable with SaaS tools (uses Slack, Asana, Google Workspace)
Not technical (doesn't code)
Uses Meta Ads Manager daily
Prefers simple, intuitive interfaces

Buying Behavior:

Tries free trials before committing
Looks for tools that save time (ROI calculator: does this save me 5+ hours/month?)
Influenced by peer recommendations and reviews
Budget approval needed from boss for tools >$100/month


Secondary User Persona: Mike, Agency Owner
Demographics:

Age: 35-50
Title: Founder, CEO, or Managing Partner
Company: Boutique agency (5-20 employees)
Location: US/Canada
Experience: 10+ years in digital marketing

Responsibilities:

Grow agency revenue and client base
Manage team of 3-15 people
Oversee all client relationships
Control costs and margins

Goals:

Scale agency without proportionally hiring more staff
Standardize deliverables across team
Increase profit margins (reduce hours per client)
Differentiate agency from competitors

Pain Points:

Can't scale because team spends too much time on operational work
Junior staff produce inconsistent quality reports
Clients churn due to lack of clear ROI demonstration
Margins shrinking due to increased operational overhead

Buying Decision:

Focused on ROI: "Does this save my team 20+ hours/month?"
Wants team buy-in before purchasing
Willing to pay for quality tools that improve client retention
Looks for annual plans to save money


Market Size & Opportunity
Total Addressable Market (TAM):

50,000+ digital marketing agencies in North America
Average agency manages Meta Ads for 10+ clients
Estimated market: $300M+ annually (50K agencies × $500/month average ad tech spend)

Serviceable Addressable Market (SAM):

15,000 agencies actively managing Meta Ads as core service
Target: Agencies with 5-50 employees
Estimated market: $90M annually

Serviceable Obtainable Market (SOM):

Target 1% of SAM in Year 1 = 150 agencies
Average contract value: $1,400/year (assume mix of Starter and Pro plans)
Year 1 Revenue Target: $210K ARR


4. PRODUCT VISION & GOALS
Vision Statement
"Empower marketing agencies to deliver world-class client reporting in seconds, not hours, so they can focus on strategy instead of operational busywork."
Product Principles
1. Simplicity First

No integrations to configure
No training required
Works in 5 minutes or less

2. Human-in-the-Loop

AI assists, humans decide
Always editable before sending to clients
Never auto-send reports (agencies review first)

3. Quality Over Quantity

One platform (Meta Ads) done excellently > 5 platforms done poorly
One report type (executive summary) done well > 10 mediocre templates

4. Agency-Centric

White-labeled by default (no "Powered by" unless agency chooses)
Agency's brand, not ours
Built for resellers, not end-clients

Goals for Year 1
Product Goals:

✅ MVP launch: Q1 2024
✅ 100 active agencies: Q2 2024
✅ Google Ads support: Q2 2024
✅ API access: Q3 2024
✅ 500 active agencies: Q4 2024

Business Goals:

$50K MRR by end of Year 1
80%+ customer retention
NPS score >40
<5% monthly churn

User Goals:

Each agency saves 15+ hours/month
90%+ of reports require no editing
4.5+ star average rating on G2/Capterra


5. SUCCESS METRICS
North Star Metric
Reports generated per month (indicates product usage and value delivery)
Target: 10,000 reports/month by end of Year 1

Key Performance Indicators (KPIs)
Product Metrics:
MetricTarget (Month 3)Target (Month 12)MeasurementMonthly Active Agencies50500Agencies generating ≥1 report/monthReports Generated/Month60010,000Total reports across all agenciesAverage Reports per Agency1220Reports/month per active agencyReport Generation Success Rate95%98%% of uploads that complete successfullyAI Analysis Quality Score3.5/54.2/5User rating after report generation
Business Metrics:
MetricTarget (Month 3)Target (Month 12)MeasurementMRR (Monthly Recurring Revenue)$5K$50KTotal subscription revenueCustomer Acquisition Cost (CAC)<$200<$150Marketing spend / new customersLifetime Value (LTV)$1,000$2,500Average revenue per customer over lifetimeLTV:CAC Ratio5:115:1Ratio of lifetime value to acquisition costMonthly Churn Rate<10%<5%% of customers canceling each monthNet Revenue Retention100%120%Includes upgrades minus downgrades
User Satisfaction Metrics:
MetricTargetMeasurementNet Promoter Score (NPS)>40Quarterly survey: "How likely to recommend?"Customer Satisfaction (CSAT)>4.5/5Post-report generation: "How satisfied?"Feature Adoption Rate>80%% of users who generate 2+ reportsTime to First Report<10 minMedian time from signup to first reportSupport Ticket Volume<5%% of users filing tickets per month

Leading vs Lagging Indicators
Leading Indicators (Predict Future Success):

Trial signups per week
Trial-to-paid conversion rate
Reports generated during trial
User engagement in first 7 days

Lagging Indicators (Measure Results):

MRR growth
Churn rate
Customer LTV
Profit margins


6. FEATURE REQUIREMENTS (FULL PRODUCT)
Core Features (MVP)

Feature 1: CSV Upload & Validation
Description:
Users upload CSV files exported from Meta Ads Manager. System validates file structure and data quality before processing.
User Story:
As an agency account manager, I want to upload my Meta Ads export CSV so that I can quickly generate a client report without manual data entry.
Acceptance Criteria:

✅ User can drag-and-drop CSV file or browse to select
✅ System validates required columns within 5 seconds
✅ Clear error messages if columns are missing or data is invalid
✅ User sees summary of validated data (campaign count, spend, date range)
✅ File size limit: 10MB
✅ Supported format: CSV only

Priority: P0 (Must-Have for MVP)
Complexity: Medium
Dependencies: None
Technical Notes:

Use PapaParse library for CSV parsing
Validation runs client-side first, then server-side
Store uploaded CSV in Supabase Storage temporarily (30 days)


Feature 2: AI-Powered Performance Analysis
Description:
AI analyzes campaign data and generates structured insights including executive summary, wins, concerns, and recommendations.
User Story:
As an agency account manager, I want AI to analyze my campaign data and write insights so that I don't spend 30 minutes manually interpreting the numbers.
Acceptance Criteria:

✅ Analysis completes in <30 seconds for typical datasets
✅ Output includes: summary (2-3 sentences), 3 wins, 2 concerns, 3 recommendations
✅ AI mentions specific campaign names (not generic)
✅ AI includes actual metrics from data (not vague statements)
✅ User can regenerate analysis if unsatisfied (up to 3 times)
✅ Analysis is editable before PDF generation

Priority: P0 (Must-Have for MVP)
Complexity: High
Dependencies: CSV validation complete
Technical Notes:

Use OpenAI GPT-4o-mini for cost efficiency
Implement strict JSON output format enforcement
Retry logic: 2 attempts with adjusted prompt if first fails
Store analysis in reports.ai_analysis (JSONB)


Feature 3: Branded PDF Report Generation
Description:
Convert AI analysis into professionally designed PDF with agency branding (logo, colors).
User Story:
As an agency owner, I want reports to include my logo and brand colors so that clients see my agency's professional brand, not a third-party tool.
Acceptance Criteria:

✅ PDF includes agency logo in header
✅ Brand color used for section headers and accents
✅ Professional typography and spacing
✅ Generates in <10 seconds
✅ File size <2MB for easy email attachment
✅ PDF is print-ready (A4 size, 150 DPI)
✅ Download link valid for 12 months

Priority: P0 (Must-Have for MVP)
Complexity: Medium
Dependencies: AI analysis complete, branding settings saved
Technical Notes:

Use @react-pdf/renderer (not Puppeteer for cost)
Upload to Supabase Storage with public URL
No charts in MVP (text and numbers only)


Feature 4: Client Management
Description:
Users can add, edit, and organize clients. Reports are associated with specific clients.
User Story:
As an agency account manager, I want to organize reports by client so that I can quickly find past reports and track which clients need reports this month.
Acceptance Criteria:

✅ User can add new client (name required, website/industry optional)
✅ User can edit client details
✅ User can delete client (with confirmation warning)
✅ Client list shows: name, last report date, total report count
✅ Clicking client shows all their reports
✅ When creating report, user selects client from dropdown

Priority: P0 (Must-Have for MVP)
Complexity: Low
Dependencies: None
Technical Notes:

Simple CRUD interface
No client portal or email notifications in MVP


Feature 5: Report History & Management
Description:
Users can view all past reports, re-download PDFs, and delete old reports.
User Story:
As an agency account manager, I want to see all past reports I've generated so that I can compare month-over-month performance or re-send a report to a client.
Acceptance Criteria:

✅ List view shows: date, client, status, actions
✅ Filter by client, date range, status
✅ Sort by date (newest first by default)
✅ Each report row has: Download PDF, View Analysis, Delete buttons
✅ Delete requires confirmation
✅ Pagination: 20 reports per page

Priority: P0 (Must-Have for MVP)
Complexity: Low
Dependencies: Report generation complete

Feature 6: Agency Branding Settings
Description:
Users configure their agency branding (logo, color) which appears on all generated reports.
User Story:
As an agency owner, I want to upload my logo and set my brand color once, and have it automatically applied to all reports.
Acceptance Criteria:

✅ User can upload logo (PNG/JPG, max 500KB)
✅ Logo preview shown after upload
✅ User can set brand color via hex input or color picker
✅ Color preview shown in real-time
✅ Settings apply to all future reports immediately
✅ User can update logo/color anytime

Priority: P0 (Must-Have for MVP)
Complexity: Low
Dependencies: None
Technical Notes:

Store logo in Supabase Storage (logos bucket)
Validate hex color format
Provide default color (#6366f1) if none set


Feature 7: Usage Tracking & Limits
Description:
System tracks how many reports user has generated this month and enforces plan limits.
User Story:
As a Starter plan user, I want to see how many reports I've used this month so that I know when I'm approaching my limit and need to upgrade or wait for reset.
Acceptance Criteria:

✅ Dashboard shows: "X of Y reports used this month"
✅ Progress bar visualization
✅ Warning when 80% of limit reached
✅ Hard block when 100% limit reached with upgrade prompt
✅ Usage resets on 1st of each month automatically
✅ Upgrade link shown when limit reached

Priority: P0 (Must-Have for MVP)
Complexity: Low
Dependencies: Subscription management
Technical Notes:

Increment agencies.reports_used_this_month after each PDF generation
Scheduled job resets counters on 1st of month


Feature 8: Subscription Management (Stripe)
Description:
Users can sign up, upgrade, downgrade, and cancel subscriptions. Payments processed via Stripe.
User Story:
As an agency owner, I want to manage my subscription (upgrade, downgrade, cancel) without contacting support so that I have control over my costs.
Acceptance Criteria:

✅ Free 14-day trial (no credit card required)
✅ After trial, user chooses Starter ($99/mo) or Pro ($199/mo)
✅ User can upgrade immediately (prorated charge)
✅ User can downgrade (takes effect next billing cycle)
✅ User can cancel anytime (access continues until end of period)
✅ User can update payment method
✅ Automatic email receipts for all charges

Priority: P0 (Must-Have for MVP)
Complexity: Medium
Dependencies: Stripe integration
Technical Notes:

Use Stripe Checkout for payment collection
Stripe webhooks for subscription events
Store stripe_customer_id and stripe_subscription_id


Post-MVP Features (Roadmap)

Feature 9: Google Ads Support (Q2 2024)
Description:
Support CSV uploads from Google Ads Manager, not just Meta Ads.
User Story:
As an agency managing both Meta and Google Ads, I want to generate reports for Google Ads clients using the same tool so I don't need multiple platforms.
Priority: P1 (High Priority Post-MVP)
Complexity: Medium
Estimated Effort: 3 weeks

Feature 10: Custom Report Templates (Q3 2024)
Description:
Agencies can customize which sections appear in reports and add custom text blocks.
User Story:
As an agency owner, I want to add a custom "Recommendations" section specific to each client's business goals so reports feel more personalized.
Priority: P1
Complexity: High
Estimated Effort: 4 weeks

Feature 11: API Access (Q3 2024)
Description:
RESTful API for programmatic report generation, enabling integrations with CRMs and other tools.
User Story:
As a technical agency owner, I want API access so I can automatically generate reports from our internal dashboard without manual CSV uploads.
Priority: P1
Complexity: High
Estimated Effort: 6 weeks

Feature 12: Team Collaboration (Q4 2024)
Description:
Multi-user accounts where team members can collaborate on reports, leave comments, and assign report creation tasks.
User Story:
As an agency owner with a 5-person team, I want my team to share access to InsightEngine so we can collaborate on client reports and track who's responsible for each client.
Priority: P2
Complexity: High
Estimated Effort: 8 weeks

Feature 13: Historical Trend Analysis (Q4 2024)
Description:
Compare current month's performance to previous months and identify trends over time.
User Story:
As an agency account manager, I want to see "CPA decreased 15% vs last month" in my reports so clients understand performance trends without me manually calculating.
Priority: P2
Complexity: High
Estimated Effort: 4 weeks

7. USER FLOWS
User Flow 1: First-Time User Onboarding
START: User signs up
    ↓
1. Enter email + password
   → Receive verification email
   → Click verification link
   ↓
2. Welcome screen
   → "Let's set up your agency branding"
   ↓
3. Upload logo (optional, can skip)
   ↓
4. Set brand color (optional, default provided)
   ↓
5. Prompt: "Add your first client"
   → Enter client name
   → Save
   ↓
6. Tutorial overlay: "Now let's create your first report!"
   → Click "New Report"
   ↓
7. Select client from dropdown
   ↓
8. Upload CSV file
   → Drag-drop or browse
   ↓
9. Validation loading (2-5 seconds)
   → Success: "12 campaigns validated, $2,450 spend"
   ↓
10. Click "Generate Analysis"
    → AI processing (15-30 seconds)
    → Loading animation with tips
    ↓
11. Analysis appears
    → User reads summary, wins, concerns, recommendations
    → (Optional) Click "Edit" to modify text
    ↓
12. Click "Generate PDF"
    → PDF rendering (5-10 seconds)
    ↓
13. Success! Download PDF
    → Celebration animation 🎉
    → "Congrats on your first report!"
    ↓
14. Prompt: "Create another report?" OR "Invite team members?"
    ↓
END: User is onboarded and has generated first report

TIME TO COMPLETE: 5-10 minutes

User Flow 2: Generating a Regular Report (Returning User)
START: User logs in
    ↓
1. Dashboard shows:
   - Reports used: 8/20
   - Recent reports list
   - "New Report" button (prominent)
   ↓
2. Click "New Report"
   ↓
3. Select existing client OR add new client
   ↓
4. Upload CSV
   → Validation (2-5 sec)
   ↓
5. Click "Generate Analysis"
   → AI processing (15-30 sec)
   ↓
6. Review analysis
   → (Optional) Edit if needed
   ↓
7. Click "Generate PDF"
   → PDF rendering (5-10 sec)
   ↓
8. Download PDF
   ↓
9. (Optional) Share via email to client directly
   ↓
END: Report generated and ready to send

TIME TO COMPLETE: 60-90 seconds (excluding editing)

User Flow 3: Handling CSV Validation Error
START: User uploads CSV
    ↓
1. Validation runs
   ↓
2. ERROR: "Missing required columns: Conversions, CPA"
   ↓
3. Error modal shows:
   - What's wrong
   - Which columns are missing
   - How to fix (link to help doc)
   ↓
4. User has options:
   → "Watch Tutorial" (video: how to export correctly)
   → "Try Different File"
   → "Contact Support"
   ↓
5. User chooses "Try Different File"
   → Goes back to Meta Ads Manager
   → Customizes columns
   → Re-exports CSV
   ↓
6. Uploads new CSV
   → Validation succeeds
   ↓
7. Continue to analysis generation
   ↓
END: User learns correct export process for future

User Flow 4: Upgrading from Trial to Paid
START: User has used 10/10 trial reports
    ↓
1. User tries to create 11th report
   ↓
2. Block screen appears:
   "Trial Limit Reached"
   - "You've used all 10 trial reports"
   - "Upgrade to continue generating reports"
   ↓
3. Show plan options:
   - Starter: $99/mo, 20 reports
   - Pro: $199/mo, 100 reports
   ↓
4. User selects plan
   ↓
5. Redirect to Stripe Checkout
   → Enter payment details
   → Billing address
   → Submit
   ↓
6. Stripe processes payment (5-10 sec)
   ↓
7. Redirect back to InsightEngine
   ↓
8. Success message: "Welcome to [Plan Name]!"
   - Reports available: 20 (or 100)
   - Next billing date: [Date]
   ↓
9. User can now create report (continue from where blocked)
   ↓
END: User is paying customer

User Flow 5: Editing AI Analysis Before PDF
START: AI analysis completed
    ↓
1. User reviews analysis
   → Summary looks good
   → Win #2 is inaccurate (wrong campaign name mentioned)
   ↓
2. Click "Edit" button next to "Wins" section
   ↓
3. Edit modal opens
   - Shows current text in textarea
   - Editable, full formatting preserved
   ↓
4. User corrects Win #2:
   - Changes "Summer Sale" to "Winter Sale"
   - Updates metric value
   ↓
5. Click "Save Changes"
   ↓
6. Modal closes
   → Updated text appears in analysis preview
   ↓
7. User reviews other sections
   → Everything else looks good
   ↓
8. Click "Generate PDF"
   → PDF includes edited text
   ↓
END: User has customized report with their expertise

8. TECHNICAL REQUIREMENTS
Performance Requirements
MetricTargetCritical ThresholdCSV Upload + Validation<5 seconds<10 secondsAI Analysis Generation<30 seconds<60 secondsPDF Generation<10 seconds<20 secondsPage Load Time (Dashboard)<2 seconds<4 secondsAPI Response Time (95th percentile)<500ms<1000msUptime99.5%99.0%

Scalability Requirements
Year 1 Targets:

500 active agencies
10,000 reports/month
100 concurrent users
50GB total storage (PDFs + CSVs)

System must handle:

50 simultaneous report generations without degradation
10x traffic spikes (e.g., end-of-month reporting surge)
Database queries returning in <100ms for typical operations


Browser Compatibility
Tier 1 Support (Full Testing):

Chrome 90+ (Desktop & Android)
Safari 14+ (Desktop & iOS)
Firefox 88+ (Desktop)
Edge 90+ (Desktop)

Tier 2 Support (Basic Testing):

Older versions of above browsers (warn user to upgrade)

Not Supported:

Internet Explorer (any version) → Show "Browser Not Supported" message


Device Support
Desktop/Laptop:

Minimum resolution: 1280×720
Optimal: 1920×1080 or higher
Touch support not required

Tablet:

Responsive layout (works but not optimized)
Upload functionality may be limited on iPad

Mobile:

Read-only experience (can view reports, can't generate)
Consider mobile app in Year 2 roadmap


Data Requirements
Data Retention:

Reports: 12 months (auto-delete after)
CSV uploads: 30 days (auto-delete after)
Account data: Indefinite (until account deletion)
Audit logs: 2 years

Backup & Recovery:

Daily automated backups
Point-in-time recovery up to 7 days
RTO (Recovery Time Objective): 4 hours
RPO (Recovery Point Objective): 24 hours


Integration Requirements
Must Integrate With:

Stripe (payments)
OpenAI API (AI analysis)
Supabase (database, auth, storage)
Postmark (transactional emails)

Future Integrations (Post-MVP):

Zapier (workflow automation)
Google Ads API (native integration)
HubSpot/Salesforce (CRM sync)


9. DESIGN REQUIREMENTS
Design Principles
1. Clarity Over Cleverness

Interface should be self-explanatory
No user should need a tutorial video
Labels and buttons use plain language

2. Speed Over Features

Fast, focused workflows
No unnecessary steps
Keyboard shortcuts for power users

3. Professional, Not Flashy

Clean, modern aesthetic
Agency-appropriate (not consumer-app playful)
Typography: Readable, professional

4. Progressive Disclosure

Show basics first, advanced options hidden
Context-sensitive help
Empty states guide next action


Visual Design Guidelines
Color Palette:
Primary:    #6366f1 (Indigo 500)
Success:    #10b981 (Green 500)
Warning:    #f59e0b (Amber 500)
Error:      #ef4444 (Red 500)
Neutral:    #111827 (Gray 900) for text
            #6b7280 (Gray 500) for secondary text
            #f9fafb (Gray 50) for backgrounds
Typography:
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
Spacing System:
4px, 8px, 12px, 16px, 24px, 32px, 48px, 64px
(Use 4px, 8px, 12px, 16px, 24px, 32px, 48px, 64px
(Use multiples of 4 for consistent rhythm)

Component padding: 16px default, 24px for cards
Section margins: 32px between major sections
Page margins: 48px on desktop, 24px on mobile
Buttons:
Primary Button:
  - Background: Primary color (#6366f1)
  - Text: White
  - Padding: 12px 24px
  - Border radius: 8px
  - Hover: Darken 10%

Secondary Button:
  - Background: Transparent
  - Border: 1px solid Gray 300
  - Text: Gray 700
  - Same padding/radius
  - Hover: Gray 50 background

Danger Button (Delete):
  - Background: Red 500
  - Text: White
  - Same padding/radius
Forms & Inputs:
Text Input:
  - Height: 44px (touch-friendly)
  - Padding: 12px 16px
  - Border: 1px solid Gray 300
  - Border radius: 8px
  - Focus: Blue ring, 2px offset

Dropdowns:
  - Same styling as text inputs
  - Chevron icon on right

File Upload:
  - Drag-drop zone: 200px height minimum
  - Dashed border (2px)
  - Hover state: Highlight with primary color
Cards & Containers:
Default Card:
  - Background: White
  - Border: 1px solid Gray 200
  - Border radius: 12px
  - Padding: 24px
  - Shadow: 0 1px 3px rgba(0,0,0,0.1)

Key Screens & Layouts
Screen 1: Dashboard (Home)
┌─────────────────────────────────────────────────────────────┐
│  [Logo]  Dashboard  Clients  Reports  Settings    [Avatar]  │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Welcome back, Sarah! 👋                                     │
│                                                               │
│  ┌─────────────────────┐  ┌────────────────────────────┐   │
│  │  Reports This Month │  │  [+ New Report] Button     │   │
│  │  15 / 20            │  │  (Large, prominent)        │   │
│  │  [Progress Bar]     │  └────────────────────────────┘   │
│  └─────────────────────┘                                     │
│                                                               │
│  Recent Reports                                              │
│  ┌───────────────────────────────────────────────────────┐  │
│  │ Jan 14  Acme Corp     Completed  [Download] [Delete] │  │
│  │ Jan 12  Beta LLC      Completed  [Download] [Delete] │  │
│  │ Jan 10  Gamma Inc     Completed  [Download] [Delete] │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                               │
│  Quick Links                                                 │
│  • View all reports                                          │
│  • Manage clients                                            │
│  • Update branding                                           │
│                                                               │
└─────────────────────────────────────────────────────────────┘

Screen 2: New Report (Upload Flow)
┌─────────────────────────────────────────────────────────────┐
│  [Back]  Create New Report                                   │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Step 1: Select Client                                       │
│  ┌───────────────────────────────────────┐                  │
│  │ [Dropdown: Select a client...      ▼] │                  │
│  └───────────────────────────────────────┘                  │
│  Or [+ Add New Client]                                       │
│                                                               │
│  ─────────────────────────────────────────                  │
│                                                               │
│  Step 2: Upload CSV Data                                     │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                                                         │  │
│  │         📁 Drag & drop CSV file here                   │  │
│  │                                                         │  │
│  │              or [Browse Files]                         │  │
│  │                                                         │  │
│  │  Supported: Meta Ads Manager exports                   │  │
│  │  Max file size: 10MB                                   │  │
│  │                                                         │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                               │
│  Need help exporting from Meta Ads?                          │
│  [Watch Tutorial Video]                                      │
│                                                               │
└─────────────────────────────────────────────────────────────┘

Screen 3: Analysis Review
┌─────────────────────────────────────────────────────────────┐
│  [Back]  Report for Acme Corp                                │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ✅ Analysis Complete!                                       │
│                                                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Executive Summary                    [Edit]        │    │
│  │  ──────────────────────────────────────────────────│    │
│  │  Campaigns generated 89 conversions at an average  │    │
│  │  CPA of $27.53, performing 8% above target         │    │
│  │  efficiency. Winter Sale drove 42% of total...     │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ✓ Key Wins                           [Edit]        │    │
│  │  ──────────────────────────────────────────────────│    │
│  │  • Winter Sale campaign achieved CPA of $18.50,    │    │
│  │    33% below target - scale immediately            │    │
│  │  • CTR of 3.2% indicates strong creative resonance│    │
│  │  • Overall conversion volume up 22%                │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ⚠ Areas of Concern                   [Edit]        │    │
│  │  ──────────────────────────────────────────────────│    │
│  │  • Summer Campaign spent $450 with 0 conversions   │    │
│  │  • Mobile CPA trending 15% higher than desktop     │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  → Recommended Actions                [Edit]        │    │
│  │  ──────────────────────────────────────────────────│    │
│  │  1. Increase budget on Winter Sale by $200/day    │    │
│  │  2. Pause Summer Campaign and audit targeting     │    │
│  │  3. Launch mobile-specific creative test          │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                               │
│  [< Try Again]            [Generate PDF >]                   │
│                                                               │
└─────────────────────────────────────────────────────────────┘

Screen 4: PDF Download Success
┌─────────────────────────────────────────────────────────────┐
│  Report Ready! 🎉                                            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Your report for Acme Corp is ready to download.            │
│                                                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  📄 Meta Ads Performance Report                       │  │
│  │     Acme Corp • January 2024                          │  │
│  │     Generated: Jan 15, 2024                           │  │
│  │     File size: 245 KB                                 │  │
│  │                                                        │  │
│  │     [Download PDF]                                    │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                               │
│  What's next?                                                │
│  • Share this report with your client                        │
│  • Generate another report for a different client            │
│  • View all your reports in the Reports tab                  │
│                                                               │
│  [Create Another Report]    [View All Reports]               │
│                                                               │
└─────────────────────────────────────────────────────────────┘

Interaction Design
Loading States:
CSV Uploading:
  - Progress bar with percentage
  - Message: "Uploading and validating your data..."

AI Analyzing:
  - Animated spinner with rotating icon
  - Messages rotate every 5 seconds:
    * "Analyzing campaign performance..."
    * "Identifying top performers..."
    * "Generating recommendations..."
  - Time estimate: "Usually takes 20-30 seconds"

PDF Generating:
  - Simple spinner
  - Message: "Creating your branded PDF..."
Empty States:
No Clients Yet:
  - Illustration (simple graphic)
  - Heading: "Add your first client"
  - Body: "Organize your reports by client for easy tracking"
  - CTA: [+ Add Client] button

No Reports Yet:
  - Illustration
  - Heading: "Ready to create your first report?"
  - Body: "Upload a CSV from Meta Ads Manager and get insights in seconds"
  - CTA: [Create Report] button
Error States:
CSV Validation Error:
  - Red alert banner
  - Clear error message
  - Specific fix instructions
  - Links to help docs
  - [Try Again] button

System Error:
  - Yellow warning banner
  - "Something went wrong. We've been notified."
  - "Please try again in a few minutes."
  - [Retry] button
  - [Contact Support] link
Success States:
Report Generated:
  - Green checkmark animation
  - Celebration message
  - Clear next steps
  - Prominent [Download] button

Settings Saved:
  - Toast notification (bottom-right corner)
  - Green background
  - "Settings saved successfully ✓"
  - Auto-dismiss after 3 seconds

10. NON-FUNCTIONAL REQUIREMENTS
Security Requirements
Authentication & Authorization:

Multi-factor authentication (MFA) optional for users
JWT tokens for API authentication
Session timeout after 7 days of inactivity
Password requirements: minimum 8 characters, mix of letters/numbers
Row Level Security (RLS) in database (users can only access their own data)

Data Protection:

All data encrypted at rest (AES-256)
All data encrypted in transit (TLS 1.3)
Sensitive data (API keys) stored in environment variables, not code
GDPR compliant (right to access, delete, export data)
CCPA compliant (California privacy law)

Input Validation:

Server-side validation on all inputs
SQL injection prevention (parameterized queries)
XSS prevention (sanitize user inputs)
CSRF protection on all state-changing operations
File upload validation (type, size, content)

Audit Logging:

Log all authentication attempts (success and failure)
Log all data access by service accounts
Log all payment transactions
Log all admin actions
Retain logs for 2 years


Reliability Requirements
Uptime:

Target: 99.5% uptime (43.8 hours downtime/year max)
Scheduled maintenance windows: Sundays 2-4 AM EST

Backup & Disaster Recovery:

Automated daily backups at 3 AM EST
Backups retained for 30 days
Point-in-time recovery available (up to 7 days)
Disaster recovery plan documented and tested quarterly

Error Handling:

Graceful degradation (if AI API down, allow manual report editing)
Automatic retry logic for transient failures (network timeouts, etc.)
User-facing error messages never expose technical details
All errors logged with stack traces for debugging


Maintainability Requirements
Code Quality:

TypeScript for all new code (type safety)
ESLint + Prettier for code formatting
Unit test coverage >70% for critical paths
Integration tests for all API endpoints
Code review required before merging to main branch

Documentation:

API documentation (OpenAPI/Swagger)
Code comments for complex logic
README files in each major directory
Onboarding guide for new developers
Architecture decision records (ADRs) for major decisions

Deployment:

CI/CD pipeline (automated testing + deployment)
Feature flags for gradual rollouts
Blue-green deployment (zero-downtime deploys)
Rollback capability within 5 minutes
Deployment frequency: Multiple times per week


Accessibility Requirements
WCAG 2.1 Level AA Compliance:

Keyboard navigation for all interactive elements
Screen reader compatible (semantic HTML, ARIA labels)
Color contrast ratio ≥4.5:1 for text
Focus indicators visible on all interactive elements
No reliance on color alone to convey information

Specific Requirements:

All images have alt text
Form inputs have associated labels
Error messages are screen-reader accessible
Video tutorials have captions
PDF reports are screen-reader friendly (text selectable)


Compliance Requirements
GDPR (General Data Protection Regulation):

✅ Right to access: Users can export all their data
✅ Right to deletion: Users can delete account + all data
✅ Right to rectification: Users can update their information
✅ Data processing agreement (DPA) with subprocessors
✅ Cookie consent banner (if using analytics cookies)

CCPA (California Consumer Privacy Act):

✅ Privacy policy clearly states what data is collected
✅ "Do Not Sell My Information" link (though we don't sell data)
✅ Right to access and delete data

SOC 2 Type II (Target: Q4 2024):

Security controls documented and audited
Annual third-party audit
Compliance report available to enterprise customers

PCI DSS (Payment Card Industry):

Not applicable (Stripe handles all card data)
We never see or store credit card numbers


11. CONSTRAINTS & ASSUMPTIONS
Constraints
Technical Constraints:

Must use Supabase for database (no migrations to other DB allowed in MVP)
Must use OpenAI API (no self-hosted AI models in MVP)
Must support only Meta Ads CSV format in MVP (Google Ads is Phase 2)
PDF generation must not exceed 20 seconds (user experience requirement)
File uploads limited to 10MB (infrastructure cost constraint)

Business Constraints:

Budget: $5K/month for infrastructure + APIs (Year 1)
Team: 2 developers + 1 designer (founding team)
Timeline: MVP must launch within 60 days from kickoff
No external fundraising in Year 1 (bootstrapped)

Resource Constraints:

OpenAI API budget: $500/month (limits # of analyses)
Storage budget: $100/month (limits # of PDFs stored)
Customer support: Founder-led (no dedicated support team yet)

Legal/Regulatory Constraints:

Must comply with GDPR/CCPA (data privacy laws)
Terms of Service must limit liability for AI-generated advice
No guarantees about AI accuracy (disclaimer required)


Assumptions
User Assumptions:

Users are comfortable with basic SaaS tools (Gmail, Slack level)
Users know how to export CSV from Meta Ads Manager
Users have reliable internet (minimum 1 Mbps)
Users primarily work on desktop/laptop (not mobile)

Market Assumptions:

Marketing agencies will pay $99-199/month for time savings
Agencies prefer CSV uploads over API integrations (for MVP)
AI quality is "good enough" even if not perfect (human review expected)
Agencies are willing to try new tools (low switching cost from manual process)

Technical Assumptions:

OpenAI API will remain stable and available (99%+ uptime)
Supabase will scale to 500 agencies without performance degradation
PDF generation library will handle all use cases (no complex charts needed)
Vercel can handle traffic spikes without issues

Business Assumptions:

10% trial-to-paid conversion rate achievable
5% monthly churn rate or lower
Word-of-mouth will drive 30%+ of signups (product-led growth)
Annual plans will represent 40% of revenue by end of Year 1


12. FUTURE ROADMAP
Q1 2024 (MVP Launch)

✅ Core features: CSV upload, AI analysis, PDF generation
✅ Subscription management (Stripe)
✅ Meta Ads support only
✅ 100 beta users

Q2 2024 (Expansion)

Google Ads support (CSV upload)
Email scheduling (send reports directly to clients)
Report templates (2-3 different layouts)
API v1.0 (read-only access)
Target: 250 active agencies

Q3 2024 (Scale)

LinkedIn Ads support
Team collaboration (multi-user accounts)
Custom branding (remove "Powered by" footer for Pro plan)
Zapier integration
Historical trend analysis (compare to previous months)
Target: 500 active agencies

Q4 2024 (Enterprise)

TikTok Ads support
White-label reseller program
Enterprise plan (custom pricing, dedicated support)
Advanced analytics (benchmark against industry averages)
Native integrations (HubSpot, Salesforce)
Target: 750 active agencies

2025 Roadmap Ideas (Not Committed)

Mobile app (iOS/Android)
Scheduled report generation (auto-generate monthly)
Client portal (clients can log in and view their reports)
Video report summaries (AI-generated Loom-style videos)
Multilingual support (Spanish, French, German)