# Design System

## Visual Guidelines

### Color Palette
*   **Primary:** Indigo-600 (`#4f46e5`) - Buttons, active states.
*   **Secondary:** Slate-900 (`#0f172a`) - Sidebar, headers.
*   **Success:** Emerald-600 (`#059669`) - Success messages, positive trends, "Good" scores.
*   **Warning:** Amber-500 (`#f59e0b`) - Concerns, alerts, "Needs Work" scores.
*   **Error:** Rose-600 (`#e11d48`) - Critical errors, destructive actions.
*   **Background:** Slate-50 (`#f8fafc`) - App background.
*   **Surface:** White (`#ffffff`) - Cards, panels.

### Typography
*   **Font Family:** Inter (Google Fonts).
*   **Headings:** Bold / SemiBold.
*   **Body:** Regular (14px).
*   **Data:** Monospace numbers for tables.
*   **Score Display:** Bold, 48px for Performance Score.

---

## Component Styles (Shadcn/UI)

### Buttons
*   **Primary:** Indigo-600 background, White text, Rounded-md.
*   **Secondary:** White background, Slate-300 border, Slate-700 text.
*   **Destructive:** Rose-600 background, White text.
*   **Ghost:** Transparent background, Slate-600 text (for "Regenerate").

### Cards
*   White background, Slate-200 border (1px), Shadow-sm, Rounded-lg.
*   Padding: 24px.

### Inputs
*   Height: 44px.
*   Border: Slate-300. Focus: Indigo-500 ring.

### Performance Score Badge (NEW)
*   **Purpose:** Answer "Is it good or bad?" at a glance.
*   **Display:** Circular badge with score (0-100) or text ("Strong", "Needs Work").
*   **Colors:**
    *   80-100: Emerald-600 background ("Strong Performance")
    *   50-79: Amber-500 background ("Moderate Performance")
    *   0-49: Rose-600 background ("Needs Attention")
*   **Placement:** Top-right of the Analysis Editor, visible before scrolling.

### Personal Note Card (NEW)
*   **Purpose:** Encourage human voice in reports. Combat "robotic AI" perception.
*   **Display:** A textarea with placeholder: "Add a personal note (e.g., 'Great month! Let's discuss scaling next week.')"
*   **Styling:** Light Indigo-50 background to stand out as optional but encouraged.
*   **Placement:** After Recommendations section, before "Forge PDF" button.

---

## Key Screens & Layouts

### 1. Dashboard Layout
*   **Sidebar (Left):** Navigation (Dashboard, Reports, Clients, Settings). Dark user profile at bottom.
*   **Header (Top):** Page Title, Global Search, Notifications.
*   **Main Content:** Centered, max-width 1200px.
*   **Trust Signal (NEW):** In empty state, show: "✓ Works every time. No debugging required."

### 2. New Report Flow (UPDATED)
*   **Step 1:** Client Select (Dropdown + "Add New" inline).
*   **Step 2:** Upload (Large dashed dropzone with helper text: "Drag your Meta Ads CSV here").
*   **Step 3:** Validation feedback (Green checklist or Red error box).
*   **Note:** Branding (logo/color) is NOT required before first report. Show default template, prompt for branding AFTER successful export.

### 3. Analysis Editor ("The Forge") (UPDATED)
*   **Header:** Client Name + Date Range + **Performance Score Badge** (e.g., "82 - Strong").
*   **Quick Summary Card (NEW):** A single, scannable paragraph at the top: "This month, [Client] spent $X and generated Y conversions at an average CPA of $Z. Overall: [Score]."
*   **Card-based layout:**
    *   **Summary:** Large text area (editable).
    *   **Wins:** 3 bullet points with editable text. Green left border.
    *   **Concerns:** 2 bullet points with editable text. Amber left border.
    *   **Recommendations:** 3 bullet points with editable text. Indigo left border.
*   **Personal Note Card (NEW):** Optional textarea for human touch.
*   **Action Bar at bottom:**
    *   "Regenerate" (Ghost button)
    *   "Preview PDF" (Secondary button) (NEW)
    *   "Forge PDF" (Primary button)

### 4. PDF Output
*   Clean, minimalist, ink-friendly.
*   Agency branding in header (uses default if not set).
*   **Performance Score** prominently displayed.
*   **Personal Note** included if provided.
*   Clear visual hierarchy (colors used sparingly for semantic meaning).

### 5. Onboarding Flow (UPDATED)
*   **Old Flow:** Signup → Branding → Add Client → First Report.
*   **New Flow (Optimized for 5-minute TTV):**
    1.  Signup (Email/Password).
    2.  "Add your first client" (Name only - 10 seconds).
    3.  Upload CSV → Generate Report (with default branding).
    4.  **Success!** "Your first report is ready. Add your logo to make it yours." (Prompt for branding AFTER first win).

---

## Accessibility (WCAG AA)
*   Keyboard navigation for all forms.
*   Focus indicators visible.
*   Contrast ratio > 4.5:1.
*   Alt text for charts/images.
*   Score badges include text labels (not color-only).

---

## Design Rationale (From Market Research)

| Feature | Why It Exists |
|:---|:---|
| **Performance Score Badge** | Research: "Clients ask 'Is it good or bad?' not 'What are the numbers?'" |
| **Quick Summary Card** | Research: "Clients prefer 1-page summaries over 10-page PDFs." |
| **Personal Note Card** | Research: "Robotic AI loses trust. Human voice matters." |
| **Deferred Branding** | Research: "5-minute time-to-value is critical for trial conversion." |
| **"No debugging" Trust Signal** | Research: "DIY Zapier hacks break. Reliability is a selling point." |
