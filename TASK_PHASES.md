# ReportForge - Development Phases & Tasks

This document contains the complete development roadmap from MVP to launch.

**Before starting any phase, ensure you've read `AI_AGENT_PROMPT.txt` for context.**

---

## PROGRESS TRACKING

Create `PROGRESS.md` and copy the checklist at the bottom of this file to track your progress.

After completing each task:
1. Check it off in `PROGRESS.md`
2. Note any issues in `ERRORS.md`
3. Commit your changes

---

# ═══════════════════════════════════════════════════════════════
# PHASE 1: FOUNDATION
# Timeline: Week 1-2
# Goal: Set up project infrastructure and core UI shell
# ═══════════════════════════════════════════════════════════════

## Task 1.1: Project Initialization

**Read First:** None (starting fresh)

**Steps:**
```
□ Initialize Next.js 14 with TypeScript
  Command: npx create-next-app@latest ./ --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"

□ Install core dependencies:
  npm install @supabase/supabase-js @supabase/ssr
  npm install openai
  npm install @react-pdf/renderer
  npm install papaparse @types/papaparse
  npm install lucide-react

□ Initialize Shadcn/UI:
  npx shadcn-ui@latest init
  (Choose: New York style, Slate base color, CSS variables: Yes)

□ Install common Shadcn components:
  npx shadcn-ui@latest add button card input label textarea
  npx shadcn-ui@latest add dialog dropdown-menu avatar
  npx shadcn-ui@latest add toast progress badge

□ Configure Tailwind with design system colors
  → Reference: docs/10-DESIGN-SYSTEM.md

□ Create .env.local with placeholders:
  NEXT_PUBLIC_SUPABASE_URL=
  NEXT_PUBLIC_SUPABASE_ANON_KEY=
  SUPABASE_SERVICE_ROLE_KEY=
  OPENAI_API_KEY=
  STRIPE_SECRET_KEY=
  STRIPE_WEBHOOK_SECRET=

□ Create PROGRESS.md and ERRORS.md files

□ Test: npm run dev - should show default Next.js page
```

**Deliverable:** Next.js project running locally with all dependencies installed.

---

## Task 1.2: Supabase Setup

**Read First:** `docs/08-SYSTEM-ARCHITECTURE.md` (Database Schema section)

**Steps:**
```
□ Create Supabase project at supabase.com

□ Create database tables using SQL editor:
  → Copy schema from docs/08-SYSTEM-ARCHITECTURE.md

□ Set up Row Level Security (RLS) policies:
  - agencies: Users can only access their own agency
  - clients: Users can only access clients in their agency
  - reports: Users can only access reports in their agency
  - subscriptions: Users can only access their subscription

□ Create Supabase client utilities:
  - src/lib/supabase/client.ts (browser client)
  - src/lib/supabase/server.ts (server client)
  - src/lib/supabase/middleware.ts (auth middleware)

□ Add Supabase credentials to .env.local

□ Test: Query empty tables from a test page
```

**Deliverable:** Supabase connected with all tables and RLS policies.

---

## Task 1.3: Authentication

**Read First:** 
- `docs/08-SYSTEM-ARCHITECTURE.md` (Auth section)
- `docs/06-PRD-FULL.md` (User Flow 1: Onboarding)

**Steps:**
```
□ Create auth pages structure:
  - src/app/(auth)/login/page.tsx
  - src/app/(auth)/signup/page.tsx
  - src/app/(auth)/callback/route.ts

□ Implement signup:
  - Email + password form
  - Supabase signUp call
  - Create agency record on signup
  - Create trial subscription record
  - Email verification (optional for MVP)

□ Implement login:
  - Email + password form
  - Supabase signInWithPassword
  - Redirect to dashboard on success
  - Error handling for invalid credentials

□ Create auth callback handler:
  - Exchange code for session
  - Redirect to dashboard

□ Create middleware for protected routes:
  - Check session on (dashboard) routes
  - Redirect to login if not authenticated

□ Implement logout:
  - Supabase signOut
  - Clear session
  - Redirect to login

□ Test: Full signup → login → logout flow
```

**Deliverable:** Users can sign up, log in, and access protected routes.

---

## Task 1.4: Dashboard Shell

**Read First:** `docs/10-DESIGN-SYSTEM.md` (Key Screens section)

**Steps:**
```
□ Create dashboard layout:
  - src/app/(dashboard)/layout.tsx

□ Implement sidebar:
  - Navigation links: Dashboard, Reports, Clients, Settings
  - Logo/brand at top
  - User profile dropdown at bottom
  - Active state highlighting
  - Collapse on mobile (hamburger menu)

□ Implement header:
  - Page title (dynamic based on route)
  - Optional: Global search, notifications

□ Create main content area:
  - Centered, max-width 1200px
  - Proper padding and spacing

□ Create dashboard home page:
  - src/app/(dashboard)/dashboard/page.tsx
  - Usage widget: "X of Y reports used this month"
  - Recent reports list (empty state for now)
  - Quick action: "New Report" button

□ Create empty state component:
  - Friendly message
  - Call to action
  - Trust signal: "✓ Works every time. No debugging required."

□ Test: Navigate between all sections, responsive design
```

**Deliverable:** Fully functional dashboard shell with navigation.

---

# ═══════════════════════════════════════════════════════════════
# PHASE 2: CLIENT MANAGEMENT
# Timeline: Week 2-3
# Goal: Users can add, edit, and manage clients
# ═══════════════════════════════════════════════════════════════

## Task 2.1: Client List Page

**Read First:** `docs/06-PRD-FULL.md` (Feature 4: Client Management)

**Steps:**
```
□ Create clients page:
  - src/app/(dashboard)/clients/page.tsx

□ Fetch clients from Supabase:
  - Filter by current user's agency_id
  - Order by name or created_at

□ Display client cards/rows:
  - Client name
  - Last report date (or "No reports yet")
  - Total report count

□ Empty state:
  - "No clients yet"
  - "Add your first client to get started"
  - Add Client button

□ Optional: Search/filter functionality

□ Test: View empty state, verify RLS is working
```

**Deliverable:** Clients page showing all clients for the agency.

---

## Task 2.2: Add Client

**Read First:** `docs/06-PRD-FULL.md` (Feature 4: Acceptance Criteria)

**Steps:**
```
□ Create AddClientDialog component:
  - Shadcn Dialog component
  - Form inside dialog

□ Form fields:
  - Name (required) - text input
  - Website (optional) - URL input
  - Industry (optional) - text input or select

□ Form validation:
  - Name is required
  - Website must be valid URL if provided

□ Submit handler:
  - Insert into Supabase clients table
  - Include agency_id from session
  - Close dialog on success
  - Show success toast

□ Refresh client list after adding

□ Test: Add client, verify appears in list
```

**Deliverable:** Users can add new clients via modal.

---

## Task 2.3: Edit/Delete Client

**Steps:**
```
□ Create EditClientDialog component:
  - Similar to AddClientDialog
  - Pre-populate with existing data
  - Update instead of insert

□ Add edit button to client row/card:
  - Opens EditClientDialog
  - Pass client data as prop

□ Create DeleteClientDialog:
  - Confirmation dialog
  - Warning about associated reports
  - Destructive button styling

□ Handle delete:
  - Soft delete or cascade delete reports?
  - For MVP: Prevent delete if reports exist, or cascade

□ Test: Edit client, delete client (handle edge cases)
```

**Deliverable:** Full CRUD for clients.

---

# ═══════════════════════════════════════════════════════════════
# PHASE 3: CSV UPLOAD & VALIDATION
# Timeline: Week 3-4
# Goal: Users can upload and validate Meta Ads CSV files
# ═══════════════════════════════════════════════════════════════

## Task 3.1: New Report Page

**Read First:** `docs/06-PRD-FULL.md` (User Flow 2: Generating a Report)

**Steps:**
```
□ Create new report page:
  - src/app/(dashboard)/reports/new/page.tsx

□ Step indicator:
  - Step 1: Select Client
  - Step 2: Upload CSV
  - Step 3: Validate
  - (Step 4: Generate - next phase)

□ Client selector:
  - Dropdown of existing clients
  - "Add New Client" option (inline creation)
  - Store selected client in state

□ Inline client creation:
  - Quick form (name only for speed)
  - Creates client and selects it

□ Test: Select client, add new client inline
```

**Deliverable:** First step of new report flow working.

---

## Task 3.2: File Upload Component

**Read First:** `docs/01-SYSTEM-PROMPTS.md` (Agent 1: CSV Validator)

**Steps:**
```
□ Create FileUploadZone component:
  - Large dashed border dropzone
  - Drag-and-drop support
  - Click to browse fallback

□ File validation (before parsing):
  - Accept only .csv files
  - Max file size: 10MB
  - Reject with clear error message

□ Upload states:
  - Idle: "Drag your Meta Ads CSV here or click to browse"
  - Dragging: Blue border highlight
  - File selected: Show file name and size
  - Uploading: Spinner/progress

□ Reset/re-upload:
  - Clear file button
  - Upload new file option

□ Test: Upload valid CSV, reject non-CSV, reject large file
```

**Deliverable:** Functional file upload component.

---

## Task 3.3: CSV Validation (Agent 1)

**Read First:** `docs/01-SYSTEM-PROMPTS.md` (Agent 1: Full Specification)

**Steps:**
```
□ Create CSV validator utility:
  - src/lib/csv/validator.ts

□ Parse CSV with PapaParse:
  - Handle headers automatically
  - Skip empty rows

□ Validate required columns (case-sensitive):
  - Campaign Name
  - Impressions
  - Clicks
  - Spend
  - Conversions
  - CPC
  - CTR
  - CPA

□ Validate data types:
  - Campaign Name: non-empty string
  - Numeric fields: valid numbers
  - CTR: between 0-100
  - CPA: positive or null (if 0 conversions)

□ Business logic validation:
  - Total spend > 0
  - At least 1 data row

□ Return validation result:
  - Success: rows count, total spend, date range
  - Failure: specific error with fix suggestion

□ Test: Valid CSV, missing columns, invalid data
```

**Deliverable:** CSV validation logic complete.

---

## Task 3.4: Validation UI Feedback

**Steps:**
```
□ Create ValidationResult component:
  - Success state (green)
  - Error state (red)

□ Success display:
  - Checkmarks for each validation step
  - Summary: "12 campaigns validated"
  - Totals: "$2,450 total spend, 89 conversions"
  - Date range if detected
  - "Generate Analysis" button (enabled)

□ Error display:
  - Red error box
  - Specific error message
  - "How to fix" suggestion
  - "Re-upload" button

□ Warning display (non-blocking):
  - Yellow warning box
  - "Low impression volume" etc.

□ Connect to Generate button:
  - Store parsed data in state
  - Enable "Generate Analysis" button

□ Test: Full upload → validation flow
```

**Deliverable:** Complete CSV upload and validation flow.

---

# ═══════════════════════════════════════════════════════════════
# PHASE 4: AI ANALYSIS
# Timeline: Week 4-5
# Goal: AI analyzes campaign data and generates insights
# ═══════════════════════════════════════════════════════════════

## Task 4.1: OpenAI Integration

**Read First:** `docs/01-SYSTEM-PROMPTS.md` (Agent 2: Full Specification)

**Steps:**
```
□ Create OpenAI client:
  - src/lib/openai/client.ts
  - Use OPENAI_API_KEY from env

□ Create analysis API route:
  - src/app/api/analyze/route.ts
  - POST method
  - Accepts: client_name, campaigns[], date_range

□ Implement Agent 2 prompt:
  - Copy system prompt from docs/01-SYSTEM-PROMPTS.md
  - Temperature: 0.3
  - Response format: JSON
  - Max tokens: 1500
  - Timeout: 30 seconds

□ Parse and validate response:
  - Check all required fields present
  - Validate performance_score is 0-100
  - Validate arrays have correct lengths

□ Error handling:
  - API timeout
  - Rate limiting
  - Invalid response format
  - Retry logic (1 retry)

□ Test with sample data:
  - Use golden dataset from docs/04-INTERACTION-TEMPLATES.md
  - Verify output matches expected format
```

**Deliverable:** OpenAI integration working with proper error handling.

---

## Task 4.2: Analysis Processing

**Read First:** `docs/01-SYSTEM-PROMPTS.md` (Data Aggregation section)

**Steps:**
```
□ Create data aggregation utility:
  - src/lib/analysis/aggregator.ts

□ Pre-process CSV data:
  - Calculate totals (spend, conversions, clicks, impressions)
  - Calculate averages (CPC, CTR, CPA)
  - Identify top 5 campaigns by conversions
  - Identify bottom 5 campaigns by CPA

□ Format for API:
  - Send aggregated stats (not full CSV)
  - Send top/bottom campaign details
  - Include client name and date range

□ Call OpenAI API:
  - Use the /api/analyze route
  - Show loading state in UI

□ Save to Supabase:
  - Create report record
  - Store all analysis fields
  - Store original CSV data (JSON)
  - Set status: 'ready'

□ Handle partial failures:
  - If analysis succeeds but save fails, retry save
  - Don't lose analysis on transient errors

□ Test: Full flow from CSV to saved report
```

**Deliverable:** End-to-end analysis pipeline working.

---

## Task 4.3: Analysis Editor ("The Forge")

**Read First:** 
- `docs/10-DESIGN-SYSTEM.md` (Analysis Editor section)
- `docs/06-PRD-FULL.md` (Feature 2: Acceptance Criteria)

**Steps:**
```
□ Create editor page:
  - src/app/(dashboard)/reports/[id]/edit/page.tsx

□ Fetch report data:
  - Get report by ID from Supabase
  - Verify user has access (agency match)

□ Performance Score Badge:
  - Circular badge with score (0-100)
  - Color based on score (green/amber/red)
  - Label: "Strong", "Moderate", "Needs Attention"
  - Position: Top right

□ Quick Summary:
  - Highlighted box below header
  - Single sentence, read-only

□ Editable sections:
  - Summary: Large textarea
  - Wins: 3 editable text inputs, green left border
  - Concerns: 2 editable text inputs, amber left border
  - Recommendations: 3 editable text inputs, indigo left border

□ Personal Note:
  - Optional textarea
  - Placeholder: "Add a personal note for your client..."
  - Light indigo background to stand out

□ Action bar:
  - "Regenerate" button (ghost style)
  - "Preview PDF" button (secondary)
  - "Forge PDF" button (primary)

□ Test: Load report, edit all fields, verify UI
```

**Deliverable:** Fully functional analysis editor.

---

## Task 4.4: Analysis State Management

**Steps:**
```
□ Track edit state:
  - Compare current values to saved values
  - Show "unsaved changes" indicator

□ Auto-save:
  - Debounce: 2 seconds after last edit
  - Save to Supabase
  - Show "Saving..." / "Saved" indicator

□ Manual save:
  - Save button (optional, since auto-save)

□ Regenerate functionality:
  - Track regeneration count (max 3)
  - Disable button after 3 regenerations
  - Show confirmation: "This will replace current analysis"
  - Call API and replace content

□ Handle concurrent edits:
  - Optimistic updates
  - Conflict resolution (last write wins for MVP)

□ Test: Auto-save, regenerate, state persistence
```

**Deliverable:** Robust state management for editor.

---

# ═══════════════════════════════════════════════════════════════
# PHASE 5: PDF GENERATION
# Timeline: Week 5-6
# Goal: Generate branded PDF reports
# ═══════════════════════════════════════════════════════════════

## Task 5.1: PDF Template

**Read First:** 
- `docs/01-SYSTEM-PROMPTS.md` (Agent 3: PDF Generator)
- `docs/10-DESIGN-SYSTEM.md` (PDF Output section)

**Steps:**
```
□ Create PDF component:
  - src/components/pdf/ReportPDF.tsx
  - Use @react-pdf/renderer

□ Page setup:
  - A4 portrait
  - 40pt margins
  - Helvetica font family

□ Header:
  - Agency logo (or text fallback)
  - Client name
  - Date range
  - Performance Score badge

□ Quick Summary:
  - Highlighted box
  - Single sentence

□ Summary Section:
  - Full analysis text

□ Wins Section:
  - Checkmark icons
  - 3 bullet points
  - Green accent

□ Concerns Section:
  - Warning triangle icons
  - 2 bullet points
  - Amber accent

□ Recommendations Section:
  - Arrow icons
  - 3 numbered items
  - Indigo accent

□ Personal Note (if provided):
  - Italicized block
  - Label: "Note from your account team"

□ Footer:
  - "Generated by [Agency Name] using ReportForge"
  - Page number

□ Test: Render PDF with sample data
```

**Deliverable:** PDF template rendering correctly.

---

## Task 5.2: Branding Integration

**Steps:**
```
□ Fetch agency branding:
  - Get logo_url and brand_color from agency record

□ Logo handling:
  - If logo_url exists, fetch and embed
  - If fetch fails, use text fallback (agency name)
  - If no logo, use agency name text

□ Brand color:
  - Use for section headers
  - Use for accent elements
  - Validate hex format
  - Fallback: #6366f1 (Indigo-600)

□ Contrast check:
  - Ensure text is readable on brand color
  - Use white or dark text based on luminance

□ Test: PDF with logo, without logo, custom colors
```

**Deliverable:** Branding applied to PDF.

---

## Task 5.3: PDF Generation API

**Steps:**
```
□ Create PDF API route:
  - src/app/api/generate-pdf/route.ts
  - POST method
  - Accepts: report_id

□ Fetch report and agency data:
  - Get report by ID
  - Get agency branding
  - Verify user access

□ Render PDF:
  - Use ReactPDF renderToBuffer
  - Handle rendering errors

□ Upload to Supabase Storage:
  - Bucket: "reports"
  - Path: {agency_id}/{report_id}.pdf
  - Content-Type: application/pdf

□ Generate signed URL:
  - Valid for 12 months
  - Return URL in response

□ Update report record:
  - Set pdf_url
  - Set status: 'complete'

□ Error handling:
  - Rendering timeout
  - Storage upload failure
  - Return appropriate errors

□ Test: Generate PDF, verify in storage
```

**Deliverable:** PDF generation API working.

---

## Task 5.4: PDF Download Flow

**Steps:**
```
□ "Forge PDF" button click:
  - Disable button
  - Show loading: "Forging your report..."

□ Call API:
  - POST to /api/generate-pdf
  - Pass report_id

□ Success handling:
  - Show success message
  - Download button with URL
  - "Open in new tab" option
  - Celebration animation (optional)

□ Error handling:
  - Show error message
  - "Try again" button
  - Log to ERRORS.md

□ Re-download:
  - If pdf_url exists, allow re-download
  - No re-generation unless edited

□ Test: Full flow from editor to downloaded PDF
```

**Deliverable:** Complete PDF generation and download flow.

---

# ═══════════════════════════════════════════════════════════════
# PHASE 6: REPORT HISTORY & SETTINGS
# Timeline: Week 6-7
# Goal: Users can view past reports and configure settings
# ═══════════════════════════════════════════════════════════════

## Task 6.1: Reports List Page

**Read First:** `docs/06-PRD-FULL.md` (Feature 5: Report History)

**Steps:**
```
□ Create reports page:
  - src/app/(dashboard)/reports/page.tsx

□ Fetch reports:
  - Filter by agency_id
  - Include client name (join)
  - Order by created_at DESC

□ Display report rows:
  - Date
  - Client name
  - Performance Score badge
  - Status (pending, ready, failed)
  - PDF download (if ready)

□ Filters:
  - By client (dropdown)
  - By date range (date picker)
  - By status (select)

□ Pagination:
  - 20 per page
  - Load more or page numbers

□ Actions:
  - View details
  - Download PDF
  - Edit analysis
  - Delete

□ Empty state:
  - "No reports yet"
  - "Create your first report" button

□ Test: List with filters, pagination
```

**Deliverable:** Reports list page complete.

---

## Task 6.2: Report Detail Page

**Steps:**
```
□ Create report detail page:
  - src/app/(dashboard)/reports/[id]/page.tsx

□ Display all analysis content (read-only):
  - Performance Score
  - Quick Summary
  - Summary
  - Wins
  - Concerns
  - Recommendations
  - Personal Note

□ Metadata:
  - Client name
  - Date range
  - Created date
  - Last modified

□ Actions:
  - Download PDF
  - Edit Analysis (go to editor)
  - Delete (with confirmation)

□ Test: View report, all actions work
```

**Deliverable:** Report detail page complete.

---

## Task 6.3: Agency Branding Settings

**Read First:** `docs/06-PRD-FULL.md` (Feature 6: Branding Settings)

**Steps:**
```
□ Create branding settings page:
  - src/app/(dashboard)/settings/branding/page.tsx

□ Logo upload:
  - File input for PNG/JPG
  - Max 500KB
  - Preview after upload
  - Upload to Supabase Storage
  - Save URL to agency record

□ Brand color:
  - Hex input with validation
  - Color picker component
  - Live preview
  - Save to agency record

□ Preview section:
  - Show sample PDF header with logo/color
  - Real-time updates

□ Save:
  - Save button
  - Success toast: "Branding updated!"

□ Test: Upload logo, change color, verify in PDF
```

**Deliverable:** Branding settings working.

---

## Task 6.4: Account Settings

**Steps:**
```
□ Create account settings page:
  - src/app/(dashboard)/settings/account/page.tsx

□ Agency name:
  - Editable text input
  - Save to agency record

□ Email change:
  - Current email display
  - New email input
  - Supabase updateUser

□ Password change:
  - Current password (optional for security)
  - New password
  - Confirm password
  - Supabase updateUser

□ Danger zone:
  - Delete account button
  - Confirmation with typed confirmation
  - Delete all data and auth account

□ Test: All settings changes work
```

**Deliverable:** Account settings complete.

---

# ═══════════════════════════════════════════════════════════════
# PHASE 7: BILLING & USAGE
# Timeline: Week 7-8
# Goal: Subscription management with Stripe
# ═══════════════════════════════════════════════════════════════

## Task 7.1: Usage Tracking

**Read First:** `docs/06-PRD-FULL.md` (Feature 7: Usage Tracking)

**Steps:**
```
□ Increment usage on report creation:
  - Update reports_used in subscriptions table
  - Do this when report status becomes 'ready'

□ Dashboard usage widget:
  - "8 of 20 reports used"
  - Progress bar
  - Color: green (0-79%), amber (80-99%), red (100%)

□ Warning at 80%:
  - Toast notification
  - Suggest upgrade

□ Block at 100%:
  - Disable "New Report" button
  - Show upgrade modal
  - Link to billing page

□ Reset on billing period:
  - When current_period_start updates
  - Reset reports_used to 0
  - (Handled by Stripe webhook)

□ Test: Generate reports, see usage update, hit limit
```

**Deliverable:** Usage tracking and limits working.

---

## Task 7.2: Stripe Integration

**Read First:** Stripe documentation for Subscriptions

**Steps:**
```
□ Create Stripe account if needed

□ Create products and prices:
  - Starter: $99/month, 20 reports
  - Pro: $199/month, 100 reports

□ Create checkout session API:
  - src/app/api/stripe/create-checkout-session/route.ts
  - Pass price_id and customer info
  - Success/cancel URLs

□ Create webhook handler:
  - src/app/api/stripe/webhook/route.ts
  - Verify webhook signature
  - Handle events:
    - checkout.session.completed
    - customer.subscription.updated
    - customer.subscription.deleted
    - invoice.paid

□ Update subscription record:
  - On checkout: create subscription record
  - On update: update plan, limits, period
  - On delete: mark cancelled

□ Test: Full checkout flow (use test mode)
```

**Deliverable:** Stripe integration complete.

---

## Task 7.3: Billing UI

**Steps:**
```
□ Create billing page:
  - src/app/(dashboard)/settings/billing/page.tsx

□ Current plan display:
  - Plan name (Trial, Starter, Pro)
  - Reports used / limit
  - Next billing date
  - Amount

□ Upgrade/downgrade:
  - Plan comparison cards
  - "Current" badge on active plan
  - Upgrade button → Stripe Checkout
  - Downgrade → Stripe Portal

□ Payment method:
  - Link to Stripe Customer Portal
  - Update card, view invoices

□ Cancel subscription:
  - Confirmation dialog
  - "You'll have access until [date]"
  - Supabase status update

□ Invoice history:
  - Link to Stripe Customer Portal

□ Test: All billing flows in test mode
```

**Deliverable:** Billing UI complete.

---

## Task 7.4: Trial Flow

**Read First:** `docs/06-PRD-FULL.md` (Feature 8: Subscription)

**Steps:**
```
□ On signup:
  - Create subscription with plan: 'trial'
  - Set reports_limit: 10 (or unlimited for trial?)
  - Set trial end date: 14 days from now

□ Trial indicator:
  - Dashboard badge: "Trial - 7 days left"
  - Color based on days remaining

□ Trial warning:
  - At 3 days: Email reminder
  - At 1 day: Toast + modal
  - At 0 days: Block access, show upgrade

□ Trial expiry:
  - Check on each page load
  - If expired and no subscription: force upgrade screen

□ Test: Signup, trial countdown, expiry handling
```

**Deliverable:** Trial flow complete.

---

# ═══════════════════════════════════════════════════════════════
# PHASE 8: POLISH & LAUNCH
# Timeline: Week 8-9
# Goal: Production-ready application
# ═══════════════════════════════════════════════════════════════

## Task 8.1: Onboarding Flow

**Read First:** `docs/06-PRD-FULL.md` (User Flow 1: Optimized Onboarding)

**Steps:**
```
□ Optimized flow (5 minutes to first report):
  1. Signup (email/password)
  2. "Who is this report for?" (add client name only)
  3. Upload CSV (drag-drop)
  4. Validate → Generate Analysis
  5. Review in editor → Forge PDF
  6. SUCCESS: Download PDF
  7. POST-SUCCESS: "Add your logo to make it yours"

□ Skip branding until after first success:
  - Use default branding for first report
  - Prompt for logo/color after first PDF

□ Progress indicator:
  - Show steps completed
  - Celebrate milestones

□ Test: Time full onboarding, target <5 minutes
```

**Deliverable:** Optimized onboarding complete.

---

## Task 8.2: Error Handling

**Steps:**
```
□ Global error boundary:
  - Catch React errors
  - Show friendly message
  - Log to console/service

□ API error handling:
  - Consistent error response format
  - User-friendly messages (no stack traces)
  - Retry suggestions where appropriate

□ Form errors:
  - Inline validation messages
  - Focus first error field
  - Accessible error announcements

□ Network errors:
  - Offline detection
  - Retry with backoff
  - "Check your connection" message

□ Log all errors:
  - Write to ERRORS.md during dev
  - Consider error tracking service (Sentry) for prod

□ Test: Trigger various errors, verify handling
```

**Deliverable:** Robust error handling throughout.

---

## Task 8.3: Performance Optimization

**Steps:**
```
□ Lazy loading:
  - PDF renderer (heavy)
  - Chart components (if any)

□ Image optimization:
  - next/image for all images
  - Proper sizing

□ Loading states:
  - Skeletons for list pages
  - Spinners for actions

□ Bundle analysis:
  - npm run build
  - Check bundle size
  - Remove unused dependencies

□ Lighthouse audit:
  - Performance > 90
  - Accessibility > 90
  - Best Practices > 90

□ Test: Fast load times, smooth interactions
```

**Deliverable:** Optimized performance.

---

## Task 8.4: Testing

**Steps:**
```
□ End-to-end user flows:
  - Signup → First report → Download PDF
  - Edit report → Re-download PDF
  - Upgrade subscription
  - Change branding

□ Cross-browser testing:
  - Chrome
  - Firefox
  - Safari (if Mac available)

□ Responsive testing:
  - Desktop (1920px, 1440px, 1280px)
  - Tablet (768px)
  - Mobile (375px)

□ Edge cases:
  - Large CSV files
  - Special characters in names
  - Long text wrapping
  - Empty states

□ Document issues in ERRORS.md
```

**Deliverable:** All flows tested and working.

---

## Task 8.5: Deployment

**Steps:**
```
□ Vercel setup:
  - Connect GitHub repo
  - Configure build settings
  - Add environment variables

□ Production environment variables:
  - Supabase production keys
  - OpenAI API key
  - Stripe live keys (when ready)

□ Domain setup (optional):
  - Custom domain
  - SSL automatic with Vercel

□ Supabase production:
  - Verify RLS policies
  - Check storage bucket permissions

□ Stripe production:
  - Switch to live keys
  - Test with real card (small amount)

□ Final verification:
  - Full signup → report flow in production
  - Check all integrations

□ Launch! 🚀
```

**Deliverable:** Production deployment complete.

---

# ═══════════════════════════════════════════════════════════════
# POST-MVP PHASES (Future Features)
# ═══════════════════════════════════════════════════════════════

## Phase 9: Google Ads Support (Q2 2026)
- Support CSV uploads from Google Ads Manager
- New validation rules for Google Ads columns
- Adapted AI prompts for Google Ads metrics

## Phase 10: Custom Templates (Q3 2026)
- Customizable sections (show/hide)
- Custom text blocks
- Drag-and-drop reordering

## Phase 11: API Access (Q3 2026)
- RESTful API for programmatic reports
- API key management
- Rate limiting and usage tracking

## Phase 12: Team Collaboration (Q4 2026)
- Multi-user accounts with roles
- Comments and annotations on reports
- Task assignment and workflow

## Phase 13: Historical Trends (Q4 2026)
- Month-over-month comparisons
- Trend charts in reports
- Performance over time visualization

---

# PROGRESS CHECKLIST

Copy this to `PROGRESS.md`:

```markdown
# ReportForge Development Progress

## Phase 1: Foundation
- [ ] Task 1.1: Project Initialization
- [ ] Task 1.2: Supabase Setup
- [ ] Task 1.3: Authentication
- [ ] Task 1.4: Dashboard Shell

## Phase 2: Client Management
- [ ] Task 2.1: Client List Page
- [ ] Task 2.2: Add Client
- [ ] Task 2.3: Edit/Delete Client

## Phase 3: CSV Upload & Validation
- [ ] Task 3.1: New Report Page
- [ ] Task 3.2: File Upload Component
- [ ] Task 3.3: CSV Validation
- [ ] Task 3.4: Validation UI Feedback

## Phase 4: AI Analysis
- [ ] Task 4.1: OpenAI Integration
- [ ] Task 4.2: Analysis Processing
- [ ] Task 4.3: Analysis Editor
- [ ] Task 4.4: Analysis State Management

## Phase 5: PDF Generation
- [ ] Task 5.1: PDF Template
- [ ] Task 5.2: Branding Integration
- [ ] Task 5.3: PDF Generation API
- [ ] Task 5.4: PDF Download Flow

## Phase 6: Report History & Settings
- [ ] Task 6.1: Reports List Page
- [ ] Task 6.2: Report Detail Page
- [ ] Task 6.3: Agency Branding Settings
- [ ] Task 6.4: Account Settings

## Phase 7: Billing & Usage
- [ ] Task 7.1: Usage Tracking
- [ ] Task 7.2: Stripe Integration
- [ ] Task 7.3: Billing UI
- [ ] Task 7.4: Trial Flow

## Phase 8: Polish & Launch
- [ ] Task 8.1: Onboarding Flow
- [ ] Task 8.2: Error Handling
- [ ] Task 8.3: Performance Optimization
- [ ] Task 8.4: Testing
- [ ] Task 8.5: Deployment
```
