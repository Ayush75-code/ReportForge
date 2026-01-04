PART 1: MODIFICATION DOCUMENT

🚨 CRITICAL: READ THIS BEFORE STARTING DEVELOPMENT
Document Purpose
This modification document supersedes certain sections of the original PRD. You MUST read this BEFORE implementing anything. The core architecture remains valid, but the positioning, messaging, and feature priorities have fundamentally changed based on market research.
Date: January 5, 2026
Status: MANDATORY - Overrides conflicting information in original PRD
Affects: Product positioning, messaging, feature priorities, UI copy

🎯 CRITICAL POSITIONING CHANGE
❌ ORIGINAL (WRONG) POSITIONING:

"InsightEngine - AI-Powered Meta Ads Reporting Tool"
"Transform CSV data into branded PDF reports in 60 seconds"

Competitive Set: AgencyAnalytics, Looker Studio, Supermetrics

✅ CORRECTED POSITIONING:

"ReportForge - AI Writing Assistant for Agency Client Reports"
"Stop writing client reports. Upload your data, we write the insights, you approve and send."

Competitive Set: Human labor, ChatGPT + manual work, Junior analysts

WHY THIS CHANGE MATTERS
Reddit market research revealed:

Agencies already have data tools - They don't need another dashboard
The pain is cognitive, not technical - Writing insights takes 2-3 hours, not pulling data
Automation promises breed mistrust - Agencies want control, not black boxes
Generic outputs kill value perception - Clients complain reports feel templated

Therefore:

We are NOT replacing reporting tools
We ARE replacing the labor of writing insights
We are NOT promising automation
We ARE promising augmentation (AI writes first draft, human approves)


📝 MANDATORY CHANGES TO IMPLEMENTATION
Change #1: Product Name
BEFORE: InsightEngine
AFTER: ReportForge
Reasoning:

"Engine" implies automation/black box
"Forge" implies crafting/creation with human control
"Report" is clearer than "Insight"

Action Required:

All branding, logos, copy use "ReportForge"
Domain: reportforge.report
Repo name: Can stay as "reportforge" or "insightengine" (doesn't matter)


Change #2: UI Copy & Messaging
CRITICAL: All user-facing text must emphasize augmentation, not automation.
Landing Page Hero (Update):
❌ WRONG:
AI-Powered Meta Ads Reports in 60 Seconds
Transform raw CSV data into branded PDF reports your clients will love.
✅ CORRECT:
Stop Writing Client Reports

Upload your Meta Ads data. We write what it means. You approve and send.

Used by agencies managing 5-50 Meta Ads clients

Dashboard Empty State (Update):
❌ WRONG:
Ready to create your first report?
Upload a CSV from Meta Ads Manager and get insights in seconds.
✅ CORRECT:
Ready to stop writing reports?
Upload your Meta Ads data. We'll write the first draft. You add the context.

Analysis Review Screen (Update):
❌ WRONG:
✅ Analysis Complete!
Your AI-powered report is ready to download.
✅ CORRECT:
✅ First Draft Ready

Review the insights below and add your client-specific context.
Your expertise makes this valuable—our AI just saves you time.

[Edit Summary] [Edit Wins] [Edit Concerns] [Edit Recommendations]

After PDF Generation (Update):
❌ WRONG:
🎉 Report Generated!
Your branded PDF is ready to download and send to your client.
✅ CORRECT:
✅ Your Report is Ready

You reviewed it. You approved it. Now send it with confidence.

[Download PDF] [Create Another Report]

Change #3: Feature Priorities (CRITICAL)
BEFORE (Wrong Priority Order):

CSV upload & validation
AI analysis
PDF generation
Multi-platform support (Google Ads Q2)
API access (Q3)

AFTER (Correct Priority Order):

CSV upload & validation
AI analysis with mandatory review step
Editing interface (NOT optional)
PDF generation
Multi-platform support DELETED until Q4 minimum
Google Sheets export (Q2) ← NEW
API access (Q3)


Change #4: Editing Flow is MANDATORY
BEFORE: Editing was optional ("click Edit if you want to change")
AFTER: Editing screen is default, always shown
Implementation:
typescript// ❌ WRONG FLOW:
Upload CSV → Validate → Generate Analysis → Show Analysis → [Download PDF]
                                                              ↓
                                                    (optional) [Edit]

// ✅ CORRECT FLOW:
Upload CSV → Validate → Generate Analysis → REVIEW & EDIT (always shown) → Generate PDF
```

**UI Requirements:**
- Analysis review page shows editable fields by default
- Each section has inline editing (not modal)
- "Generate PDF" button only appears after user has seen the analysis
- Add helper text: "Add your client-specific context before generating PDF"

---

### **Change #5: Remove "Automation" Language**

**Find and replace these terms everywhere:**

| ❌ DON'T USE | ✅ USE INSTEAD |
|-------------|---------------|
| "Automated reporting" | "AI-assisted report writing" |
| "AI-powered reports" | "AI-written first drafts" |
| "Generate insights automatically" | "We write the insights, you approve" |
| "Instant reports" | "First draft in 30 seconds" |
| "Auto-generate" | "Draft generation" |
| "Automation" | "Augmentation" or "Assistant" |

**Files to update:**
- All marketing copy (landing page, emails)
- All UI text (buttons, helper text, empty states)
- Documentation
- README.md

---

### **Change #6: Single Platform Focus (MVP)**

**BEFORE:** 
- Meta Ads (Q1)
- Google Ads (Q2)
- LinkedIn Ads (Q3)
- TikTok Ads (Q4)

**AFTER:**
- Meta Ads ONLY until 100 paying customers
- No Google Ads until Q4 minimum
- Focus on quality over breadth

**Reasoning from Reddit:**
> "The proposal was just a blanket document with our details inserted basically."

**Multi-platform = generic outputs = no value perception**

**Action Required:**
- Remove all Google Ads references from MVP
- Remove "coming soon: Google Ads" from UI
- AI prompts should be Meta Ads-specific
- Use Meta Ads terminology throughout

---

## **🎯 UPDATED VALUE PROPOSITION**

### **For Marketing Materials:**

**Headline:**
> Stop Writing Client Reports

**Subheadline:**
> Upload your Meta Ads data. We write what it means. You approve and send.

**Value Props (in order):**
1. **Save 2-3 hours per client** ← Time savings
2. **Your expertise + our AI** ← Emphasizes control
3. **Clients get clear insights, not just numbers** ← Outcome
4. **White-labeled with your branding** ← Professional

**NOT:**
- ❌ "60 seconds" (speed is not primary pain)
- ❌ "AI-powered" (sounds like hype)
- ❌ "Automated" (breeds mistrust)
- ❌ "Better than dashboards" (wrong competitive set)

---

## **💰 UPDATED PRICING LANGUAGE**

### **BEFORE (Wrong Framing):**
```
Starter Plan - $99/month
20 reports per month

Pro Plan - $199/month
100 reports per month
```

### **AFTER (Correct Framing):**
```
Starter Plan - $99/month
20 clients per month
Save 40 hours of report writing

Pro Plan - $199/month
100 clients per month
Save 200 hours of report writing
```

**Add ROI Calculator:**
```
Without ReportForge:
20 clients × 2 hours = 40 hours/month
40 hours × $100/hour = $4,000 in labor cost

With ReportForge:
$99/month

You save: $3,901/month (3,933% ROI)

📊 UPDATED SUCCESS METRICS
BEFORE (Wrong Metrics):

Reports generated per month
Average generation time
CSV validation success rate

AFTER (Correct Metrics):
Primary Metrics:

Hours saved per agency per month ← Core value
Edit rate (% of reports edited) ← Engagement indicator
Repeat usage rate ← Stickiness

Secondary Metrics:

"Worth it" score (survey: "Is ReportForge worth the cost?")
Time from signup to first report sent to client
Customer acquisition cost vs lifetime value


🚨 CRITICAL: What NOT to Change
The following from the original PRD remains correct and should NOT be changed:
✅ Technical Architecture

Next.js 14, Supabase, OpenAI API, react-pdf
Database schema
API structure
File organization

✅ Core Features

CSV upload & validation
AI analysis generation
PDF generation with branding
Client management
Usage tracking
Stripe integration

✅ Security & Compliance

Row Level Security
GDPR compliance
Data encryption
Audit logging

✅ Development Timeline

60-day MVP timeline
Week-by-week breakdown
Milestones

ONLY positioning, messaging, and feature priorities have changed.

📋 IMPLEMENTATION CHECKLIST
Before writing ANY code, verify:

 All references to "InsightEngine" changed to "ReportForge"
 All UI copy emphasizes "augmentation" not "automation"
 Analysis review screen is mandatory, not optional
 Editing interface is always shown before PDF generation
 Landing page hero uses new messaging
 Pricing page shows "clients per month" not "reports per month"
 Google Ads removed from MVP scope
 No "AI-powered" or "automation" language in user-facing text
 Helper text emphasizes "you control the output"


🔄 HOW TO USE THIS DOCUMENT

Read this BEFORE the original PRD
When conflicts arise, this document takes precedence
Original PRD technical architecture remains valid
Original PRD messaging/positioning is overridden by this document

Think of it this way:

Original PRD = Technical blueprint (still correct)
This document = Market positioning correction (supersedes messaging)


❓ WHEN TO ESCALATE
If you encounter situations where this modification document conflicts with original PRD technical requirements, STOP and escalate:

Document the conflict in ERRORS.md
Note which section of original PRD conflicts
Ask for clarification before proceeding

Example conflicts to escalate:

"Should analysis editing be optional or mandatory?" → Mandatory per this doc
"Should we build Google Ads support in Q2?" → No per this doc
"Should we call it automation?" → No per this doc


✅ SUMMARY: Key Changes
AspectBEFOREAFTERProduct NameInsightEngineReportForgePositioningReporting toolWriting assistantCompetitive SetDashboards/toolsHuman laborPrimary BenefitSpeed (60 seconds)Cognitive relief (2-3 hours saved)Messaging"Automation""Augmentation"EditingOptionalMandatoryPlatform SupportMulti-platform Q1-Q4Meta Ads only until PMFSuccess MetricReports generatedHours saved

This modification document is complete. 