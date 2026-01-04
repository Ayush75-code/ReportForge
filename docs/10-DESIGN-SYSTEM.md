# Design System

## 9. DESIGN REQUIREMENTS

### Visual Guidelines

**Color Palette**
*   **Primary:** Indigo-600 (`#4f46e5`) - Buttons, active states.
*   **Secondary:** Slate-900 (`#0f172a`) - Sidebar, headers.
*   **Success:** Emerald-600 (`#059669`) - Success messages, positive trends.
*   **Warning:** Amber-500 (`#f59e0b`) - Concerns, alerts.
*   **Error:** Rose-600 (`#e11d48`) - Critical errors, destructive actions.
*   **Background:** Slate-50 (`#f8fafc`) - App background.
*   **Surface:** White (`#ffffff`) - Cards, panels.

**Typography**
*   **Font Family:** Inter (Google Fonts).
*   **Headings:** Bold / SemiBold.
*   **Body:** Regular (14px).
*   **Data:** Monospace numbers for tables.

### Component Styles (Shadcn/UI)

**Buttons**
*   **Primary:** Indigo-600 background, White text, Rounded-md.
*   **Secondary:** White background, Slate-300 border, Slate-700 text.
*   **Destructive:** Rose-600 background, White text.

**Cards**
*   White background, Slate-200 border (1px), Shadow-sm, Rounded-lg.
*   Padding: 24px.

**Inputs**
*   Height: 44px.
*   Border: Slate-300. Focus: Indigo-500 ring.

### Key Screens & Layouts

**1. Dashboard Layout**
*   **Sidebar (Left):** Navigation (Dashboard, Reports, Clients, Settings). Dark user profile at bottom.
*   **Header (Top):** Page Title, Global Search, Notifications.
*   **Main Content:** Centered, max-width 1200px.

**2. New Report Flow**
*   **Step 1:** Client Select (Dropdown).
*   **Step 2:** Upload (Large dashed dropzone).
*   **Step 3:** Validation feedback (Green checklist or Red error box).

**3. Analysis Editor ("The Forge")**
*   Card-based layout.
*   **Summary:** Large text area.
*   **Wins/Concerns:** Bullets with editable text.
*   Action Bar at bottom: "Regenerate" (Ghost), "Forge PDF" (Primary).

**4. PDF Output**
*   Clean, minimalist, ink-friendly.
*   Agency branding in header.
*   Clear visual hierarchy (colors used sparingly for semantic meaning).

### Accessibility (WCAG AA)
*   Keyboard navigation for all forms.
*   Focus indicators visible.
*   Contrast ratio > 4.5:1.
*   Alt text for charts/images.
