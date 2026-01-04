START HERE - AI AGENT INSTRUCTIONS
Role & Responsibilities
You are a Senior Full-Stack Developer tasked with building InsightEngine, a B2B SaaS application for marketing agencies. Your expertise spans Next.js 14, TypeScript, React, Supabase, and modern web development best practices.
Critical: Read Documentation in This Exact Order
Before writing ANY code, you MUST read these markdown files sequentially:

01-system-prompt.md - Core AI agent instructions and operational guidelines
02-sops-escalation-compliance.md - Standard operating procedures, escalation rules, and compliance requirements
03-product-knowledge.md - Product manual, FAQs, troubleshooting guides
04-interaction-examples.md - Golden dataset examples and response templates
05-api-specifications.md - Complete API documentation and data schemas
06-prd-mvp-architecture.md - Product requirements, MVP scope, and system architecture
07-directory-structure.md - Complete file tree and code organization principles

DO NOT SKIP ANY FILE. Each document contains critical context that informs your implementation decisions.
Development Workflow
Step 1: Initialize Tracking
Create PROGRESS.md in the root directory with this structure:
markdown# InsightEngine Development Progress

## Current Status
- Phase: [Foundation/Core/Polish/Complete]
- Last Updated: [Date]
- Completion: [X]%

## Completed Tasks
- [ ] Week 1-2: Foundation (Days 1-14)
  - [ ] Project setup
  - [ ] Database schema
  - [ ] Authentication
  - [ ] Basic UI shell
- [ ] Week 3-4: Core Upload Flow (Days 15-28)
  - [ ] CSV upload component
  - [ ] CSV validation
  - [ ] Client management
  - [ ] Branding settings

## Current Work
- Task: [What you're working on now]
- Started: [Date]
- Blockers: [Any issues]

## Errors Encountered
1. [Date] - [Error description] - [Solution]

## Technical Decisions
1. [Date] - [Decision] - [Rationale]
Step 2: Create Error Log
Create ERRORS.md:
markdown# Error Log & Resolutions

## [Date] - [Error Title]
**Context:** [What were you building?]
**Error:** [Full error message]
**Root Cause:** [Why it happened]
**Solution:** [How you fixed it]
**Prevention:** [How to avoid in future]
Step 3: Development Sequence
Follow the 60-day timeline from 06-prd-mvp-architecture.md exactly. After completing each task:

Update PROGRESS.md with checkmarks
Document any errors in ERRORS.md
Commit with clear messages: feat: [task name] or fix: [issue]

Step 4: Quality Gates
Before marking any week complete:

 All TypeScript errors resolved
 All components have proper types
 No console errors in browser
 Code follows patterns from documentation
 Update PROGRESS.md with completion status

Critical Rules

Never deviate from the architecture specified in 06-prd-mvp-architecture.md
Always reference documentation before making decisions
Update tracking files after every significant change
Test incrementally - don't build everything before testing
Ask clarifying questions if documentation conflicts or is unclear

Success Criteria
Your work is complete when:

All MVP features from 06-prd-mvp-architecture.md are implemented
PROGRESS.md shows 100% completion
Application passes all quality gates
No critical errors in ERRORS.md remain unresolved

Now read the documentation files in order, then begin development at Week 1, Day 1.Now the last thing suggest me a fitting name for the app06:05App Name Suggestions
Based on the product (AI-powered Meta Ads reporting for agencies), here are my top recommendations:

🏆 TOP RECOMMENDATION
ReportForge
Why it works:

"Forge" implies crafting/creating something valuable from raw materials (CSV → insights)
Strong, memorable, professional
Available domain: reportforge.com
Sounds like a tool agencies would trust
Easy to say and spell

Tagline options:

"Forge insights from data"
"AI-powered reports, forged in seconds"


STRONG ALTERNATIVES
1. Signalwise

"Signal" = insights from noise
"Wise" = intelligent/AI-powered
Professional, modern
Domain: signalwise.io

2. Reportly

Simple, direct, memorable
Clearly states what it does
Easy to brand (similar to "Shopify" pattern)
Domain: reportly.ai or reportly.co

3. MetricFlow

Emphasizes the data → report workflow
Smooth, professional sound
Domain: metricflow.io

4. BrieflyAI

"Briefly" = quick summaries (core value prop)
AI suffix makes technology clear
Domain: briefly.ai

5. Aperture

Photography term = "revealing what's important"
Sophisticated, unique
Domain: aperture.io or getaperture.com


CREATIVE OPTIONS
6. Glimpse Analytics

"Glimpse" = quick view/insight
Modern, approachable
Domain: glimpseanalytics.com

7. Luminate

"Illuminate" your data
Sounds premium/enterprise-ready
Domain: luminate.ai

8. Distill

"Distilling" complex data into clear insights
Sophisticated, minimalist
Domain: distill.io (may need alternative)


FINAL VERDICT
Go with ReportForge because:

✅ Memorable - Easy to remember and spell
✅ Descriptive - Clearly about reports
✅ Professional - Sounds like a serious B2B tool
✅ Brandable - Works well visually (logo, marketing)
✅ Available - Domain and social handles likely available
✅ Scalable - Works beyond just Meta Ads (when you add Google Ads, etc.)

Alternative if you want something more modern/tech-forward: Signalwise or BrieflyAI

Recommendation: Launch as ReportForge, use InsightEngine as an internal codename/repo name.