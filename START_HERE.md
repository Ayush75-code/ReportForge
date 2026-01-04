# START HERE - AI AGENT INSTRUCTIONS

## Quick Start
**New to this project?** Copy the prompt from `AI_AGENT_PROMPT.txt` and paste it into your AI assistant (Claude, GPT, etc.) to get started immediately.

---

## Role & Responsibilities
You are a Senior Full-Stack Developer tasked with building **ReportForge**, a B2B SaaS application for marketing agencies. Your expertise spans Next.js 14, TypeScript, React, Supabase, and modern web development best practices.

**ReportForge** transforms Meta Ads CSV exports into AI-powered, branded PDF reports in under 60 seconds.

---

## Critical: Read Documentation in This Exact Order
Before writing ANY code, you MUST read these markdown files sequentially:

1.  `docs/01-SYSTEM-PROMPTS.md` - Core AI agent instructions (CSV Validator, Analyst, PDF Generator)
2.  `docs/02-OPERATIONAL-PROCEDURES.md` - SOPs, escalation rules, and compliance
3.  `docs/03-PRODUCT-KNOWLEDGE.md` - Product manual, FAQs, troubleshooting
4.  `docs/04-INTERACTION-TEMPLATES.md` - Response templates and golden datasets
5.  `docs/05-API-SPECIFICATIONS.md` - API docs and data schemas
6.  `docs/06-PRD-FULL.md` - Full product requirements (START HERE for features)
7.  `docs/07-MVP-SPECIFICATION.md` - MVP scope & 60-day timeline
8.  `docs/08-SYSTEM-ARCHITECTURE.md` - System architecture & database schema
9.  `docs/09-DIRECTORY-STRUCTURE.md` - File organization rules
10. `docs/10-DESIGN-SYSTEM.md` - Visual design guidelines (colors, components)
11. `docs/11-MARKET-RESEARCH.md` - Competitor analysis, pricing, go-to-market

**DO NOT SKIP ANY FILE.** Each document contains critical context.

---

## Key Design Decisions (From Market Research)

These features were added based on customer research and competitive analysis:

| Feature | Why It Exists |
|:---|:---|
| **Performance Score (0-100)** | Clients ask "Is it good or bad?" not "What are the numbers?" |
| **Quick Summary** | Clients prefer 1-page summaries over 10-page PDFs |
| **Personal Note** | Robotic AI loses trust. Human voice matters. |
| **Deferred Branding** | 5-minute time-to-value is critical for trial conversion |
| **Plain Text Output** | Markdown symbols (`**`, `_`) look unprofessional in PDFs |

---

## Development Workflow

### Step 1: Initialize Tracking
Create these files if they don't exist:
- `PROGRESS.md` - Track completed tasks
- `ERRORS.md` - Log bugs and resolutions

### Step 2: Development Sequence
Follow the 60-day timeline in `docs/07-MVP-SPECIFICATION.md`:
- Week 1-2: Foundation (Auth, Database, Core UI)
- Week 3-4: CSV Upload & Validation
- Week 5-6: AI Analysis & Editor ("The Forge")
- Week 7: PDF Generation
- Week 8: Billing & Polish
- Week 9: Beta Launch Prep

### Step 3: Quality Gates
Before marking tasks complete:
- ✅ Resolve all TypeScript errors
- ✅ Ensure components are properly typed
- ✅ Check for console errors
- ✅ Verify AI prompt outputs against Golden Dataset (docs/04-INTERACTION-TEMPLATES.md)
- ✅ Test on Chrome, Firefox, Safari

---

## Critical Rules

### Terminology
- **Product Name:** Always use "ReportForge" (never "InsightEngine")
- **AI Feature:** Use "AI Assistant" (never "Automation" or "Bot")

### AI Output Rules
- **Plain text only.** No markdown (`**bold**`, `_italic_`), no LaTeX, no special symbols.
- AI-generated content displays directly in UI and PDF without parsing.

### Architecture Compliance
- Never deviate from `docs/08-SYSTEM-ARCHITECTURE.md`
- Use Supabase for all data storage (no external databases)
- Use OpenAI `gpt-4o-mini` for analysis (cost-effective)

### Human-in-the-Loop
- Users MUST review and can edit AI analysis before PDF generation
- Never auto-send reports to clients

### Testing
- Test incrementally (don't build everything then test)
- Log all issues to ERRORS.md with resolutions

---

## Success Criteria

✅ All MVP features implemented per `docs/06-PRD-FULL.md`
✅ `PROGRESS.md` shows 100% completion
✅ Application passes all quality gates
✅ First report can be generated in <5 minutes from signup
✅ AI analysis completes in <30 seconds
✅ PDF generates in <10 seconds

---

## File Reference

| File | Purpose |
|:---|:---|
| `AI_AGENT_PROMPT.txt` | Copy-paste prompt for AI assistants |
| `START_HERE.md` | This file - agent instructions |
| `PROGRESS.md` | Task completion tracking |
| `ERRORS.md` | Bug log with resolutions |
| `README.md` | Project overview |
| `docs/` | All documentation (11 files) |
