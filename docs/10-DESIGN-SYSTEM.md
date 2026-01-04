# Design System

ReportForge uses a **clean, professional, modern SaaS theme** with a light background, subtle depth, and indigo accents.

---

## Theme Overview

**Style:** Clean, Professional, Modern SaaS
**Mode:** Light mode only (MVP)
**Feel:** Trustworthy, efficient, premium but approachable
**Inspiration:** Linear, Notion, Vercel Dashboard

---

## Color Palette

### Primary Colors
| Name | Hex | Tailwind | Usage |
|:---|:---|:---|:---|
| **Primary** | `#4f46e5` | `indigo-600` | Buttons, links, active states, brand accent |
| **Primary Hover** | `#4338ca` | `indigo-700` | Button hover states |
| **Primary Light** | `#e0e7ff` | `indigo-100` | Subtle backgrounds, badges |
| **Primary Dark** | `#3730a3` | `indigo-800` | Pressed states |

### Neutral Colors
| Name | Hex | Tailwind | Usage |
|:---|:---|:---|:---|
| **Background** | `#f8fafc` | `slate-50` | Page background |
| **Surface** | `#ffffff` | `white` | Cards, modals, panels |
| **Border** | `#e2e8f0` | `slate-200` | Card borders, dividers |
| **Border Strong** | `#cbd5e1` | `slate-300` | Input borders |
| **Text Primary** | `#0f172a` | `slate-900` | Headings, primary text |
| **Text Secondary** | `#475569` | `slate-600` | Body text, descriptions |
| **Text Muted** | `#94a3b8` | `slate-400` | Placeholders, hints |

### Semantic Colors
| Name | Hex | Tailwind | Usage |
|:---|:---|:---|:---|
| **Success** | `#059669` | `emerald-600` | Success states, wins, strong performance |
| **Success Light** | `#d1fae5` | `emerald-100` | Success backgrounds |
| **Warning** | `#f59e0b` | `amber-500` | Warnings, concerns, moderate performance |
| **Warning Light** | `#fef3c7` | `amber-100` | Warning backgrounds |
| **Error** | `#e11d48` | `rose-600` | Errors, destructive actions, needs attention |
| **Error Light** | `#ffe4e6` | `rose-100` | Error backgrounds |

### Sidebar Colors
| Name | Hex | Tailwind | Usage |
|:---|:---|:---|:---|
| **Sidebar BG** | `#0f172a` | `slate-900` | Sidebar background (dark) |
| **Sidebar Text** | `#e2e8f0` | `slate-200` | Sidebar text |
| **Sidebar Active** | `#1e293b` | `slate-800` | Active nav item background |
| **Sidebar Hover** | `#334155` | `slate-700` | Hover state |

---

## Typography

### Font Family
```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```

**Google Fonts Import:**
```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

### Type Scale
| Name | Size | Weight | Line Height | Usage |
|:---|:---|:---|:---|:---|
| **Display** | 48px (3rem) | 700 (Bold) | 1.1 | Performance Score number |
| **H1** | 30px (1.875rem) | 700 (Bold) | 1.2 | Page titles |
| **H2** | 24px (1.5rem) | 600 (SemiBold) | 1.3 | Section headings |
| **H3** | 20px (1.25rem) | 600 (SemiBold) | 1.4 | Card titles |
| **H4** | 16px (1rem) | 600 (SemiBold) | 1.5 | Subsection titles |
| **Body** | 14px (0.875rem) | 400 (Regular) | 1.6 | Body text, descriptions |
| **Body Large** | 16px (1rem) | 400 (Regular) | 1.6 | Important body text |
| **Small** | 12px (0.75rem) | 400 (Regular) | 1.4 | Captions, hints |
| **Tiny** | 11px (0.6875rem) | 500 (Medium) | 1.4 | Badges, labels |

### Text Colors
- **Headings:** `slate-900` (primary text)
- **Body:** `slate-600` (secondary text)
- **Muted:** `slate-400` (placeholders, hints)
- **Links:** `indigo-600` (underline on hover)

### AI-Generated Text
- Rendered as **plain text only**
- No markdown parsing
- If `**`, `_`, `#` appear, they display as literal characters

---

## Spacing System

Uses 4px base unit:

| Token | Value | Usage |
|:---|:---|:---|
| `space-1` | 4px | Tiny gaps |
| `space-2` | 8px | Tight spacing |
| `space-3` | 12px | Small gaps |
| `space-4` | 16px | Default gap |
| `space-5` | 20px | Medium gap |
| `space-6` | 24px | Card padding |
| `space-8` | 32px | Section gaps |
| `space-10` | 40px | Large gaps |
| `space-12` | 48px | Section margins |
| `space-16` | 64px | Page margins |

---

## Border Radius

| Token | Value | Usage |
|:---|:---|:---|
| `rounded-sm` | 4px | Badges, small elements |
| `rounded-md` | 6px | Buttons, inputs |
| `rounded-lg` | 8px | Cards, modals |
| `rounded-xl` | 12px | Large cards |
| `rounded-full` | 9999px | Avatars, circular badges |

---

## Shadows

| Token | Value | Usage |
|:---|:---|:---|
| `shadow-sm` | `0 1px 2px rgba(0,0,0,0.05)` | Cards (default) |
| `shadow` | `0 1px 3px rgba(0,0,0,0.1), 0 1px 2px rgba(0,0,0,0.06)` | Dropdowns |
| `shadow-md` | `0 4px 6px rgba(0,0,0,0.1)` | Modals, elevated cards |
| `shadow-lg` | `0 10px 15px rgba(0,0,0,0.1)` | Dialogs, overlays |

---

## Component Styles

### Buttons

**Primary Button:**
```css
background: #4f46e5;     /* indigo-600 */
color: white;
padding: 10px 16px;
border-radius: 6px;
font-weight: 500;
font-size: 14px;
transition: background 150ms;
/* Hover */
background: #4338ca;     /* indigo-700 */
/* Focus */
ring: 2px indigo-500 offset-2;
```

**Secondary Button:**
```css
background: white;
color: #334155;          /* slate-700 */
border: 1px solid #e2e8f0; /* slate-200 */
/* Hover */
background: #f8fafc;     /* slate-50 */
```

**Destructive Button:**
```css
background: #e11d48;     /* rose-600 */
color: white;
/* Hover */
background: #be123c;     /* rose-700 */
```

**Ghost Button:**
```css
background: transparent;
color: #475569;          /* slate-600 */
/* Hover */
background: #f1f5f9;     /* slate-100 */
```

### Cards

```css
background: white;
border: 1px solid #e2e8f0; /* slate-200 */
border-radius: 8px;
padding: 24px;
box-shadow: 0 1px 2px rgba(0,0,0,0.05);
```

**Card with accent (Wins section):**
```css
border-left: 4px solid #059669; /* emerald-600 */
```

**Card with accent (Concerns section):**
```css
border-left: 4px solid #f59e0b; /* amber-500 */
```

**Card with accent (Recommendations section):**
```css
border-left: 4px solid #4f46e5; /* indigo-600 */
```

### Inputs

```css
height: 44px;
padding: 10px 12px;
border: 1px solid #cbd5e1; /* slate-300 */
border-radius: 6px;
font-size: 14px;
/* Focus */
border-color: #4f46e5;   /* indigo-600 */
ring: 2px indigo-100;
/* Placeholder */
color: #94a3b8;          /* slate-400 */
```

### Performance Score Badge

**Circle badge showing 0-100 score:**

```css
/* Container */
width: 80px;
height: 80px;
border-radius: 50%;
display: flex;
align-items: center;
justify-content: center;
flex-direction: column;

/* Score 80-100: Strong */
background: #059669;     /* emerald-600 */
color: white;

/* Score 50-79: Moderate */
background: #f59e0b;     /* amber-500 */
color: white;

/* Score 0-49: Needs Attention */
background: #e11d48;     /* rose-600 */
color: white;

/* Score text */
font-size: 28px;
font-weight: 700;

/* Label text */
font-size: 10px;
text-transform: uppercase;
opacity: 0.9;
```

### Quick Summary Card

```css
background: #f0f9ff;     /* sky-50 */
border: 1px solid #bae6fd; /* sky-200 */
border-radius: 8px;
padding: 16px 20px;
margin-bottom: 24px;
```

### Personal Note Card

```css
background: #eef2ff;     /* indigo-50 */
border: 1px solid #c7d2fe; /* indigo-200 */
border-radius: 8px;
padding: 16px;
```

---

## Layout

### Page Structure

```
┌──────────────────────────────────────────────────────┐
│  Sidebar (240px)  │         Main Content             │
│  [Dark: slate-900]│  ┌────────────────────────────┐  │
│                   │  │  Header (Page Title)       │  │
│  ○ Dashboard      │  ├────────────────────────────┤  │
│  ○ Reports        │  │                            │  │
│  ○ Clients        │  │     Content Area           │  │
│  ○ Settings       │  │     (max-width: 1200px)    │  │
│                   │  │     (centered)             │  │
│  ───────────────  │  │                            │  │
│  [User Avatar]    │  │                            │  │
│  User Name        │  │                            │  │
└──────────────────────────────────────────────────────┘
```

### Sidebar
- Width: 240px (fixed)
- Background: `slate-900` (dark)
- Padding: 16px
- Logo at top
- Navigation links
- User dropdown at bottom

### Main Content
- Background: `slate-50`
- Padding: 32px
- Max-width: 1200px (centered)

### Header
- Height: 64px
- Background: white
- Border-bottom: 1px `slate-200`
- Contains: Page title, actions

---

## Key Screens

### 1. Dashboard
- **Usage Widget:** Card showing "8 of 20 reports used" with progress bar
- **Recent Reports:** Table/list of recent reports
- **Quick Action:** Large "New Report" button
- **Empty State:** Friendly illustration + "Create your first report"

### 2. New Report Flow
- **Step Indicator:** 3 steps (Select Client → Upload CSV → Generate)
- **Upload Zone:** Dashed border, 200px height, drag-drop
- **Validation:** Green checkmarks or red error with fix suggestion

### 3. Analysis Editor ("The Forge")
- **Header:** Client name, date range, Performance Score Badge
- **Quick Summary:** Highlighted box at top
- **Sections:** Summary, Wins (green), Concerns (amber), Recommendations (indigo)
- **Personal Note:** Optional indigo-tinted textarea
- **Action Bar:** Regenerate, Preview PDF, Forge PDF

### 4. Reports List
- **Table View:** Date, Client, Score, Status, Actions
- **Filters:** By client, date range
- **Pagination:** 20 per page

### 5. Settings
- **Tabs:** Branding, Account, Billing
- **Branding:** Logo upload, color picker with preview

---

## Animation & Transitions

**Default transition:**
```css
transition: all 150ms ease-in-out;
```

**Button hover:** 150ms
**Card hover:** Slight shadow increase (optional)
**Modal open:** Fade in 200ms + scale from 95%
**Toast notifications:** Slide in from right
**Loading states:** Subtle pulse animation

---

## Accessibility (WCAG AA)

- **Contrast:** All text meets 4.5:1 ratio
- **Focus:** Visible focus rings on all interactive elements
- **Keyboard:** Full keyboard navigation
- **Screen readers:** Proper ARIA labels
- **Color:** Never use color alone (always include text/icons)
- **Motion:** Respect `prefers-reduced-motion`

---

## Design Rationale (From Market Research)

| Feature | Why It Exists |
|:---|:---|
| **Performance Score Badge** | Clients ask "Is it good or bad?" not "What are the numbers?" |
| **Quick Summary Card** | Clients prefer 1-page summaries over 10-page PDFs |
| **Personal Note Card** | Robotic AI loses trust. Human voice matters. |
| **Deferred Branding** | 5-minute time-to-value is critical for trial conversion |
| **"No debugging" Trust Signal** | DIY Zapier hacks break. Reliability is a selling point. |
| **Dark Sidebar** | Professional appearance, commonly used in SaaS dashboards |
| **Indigo Primary** | Modern, trustworthy, stands out from red/blue competitors |

---

## Tailwind Config

Add this to `tailwind.config.js` for the design system:

```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eef2ff',
          100: '#e0e7ff',
          200: '#c7d2fe',
          300: '#a5b4fc',
          400: '#818cf8',
          500: '#6366f1',
          600: '#4f46e5',  // Main primary
          700: '#4338ca',
          800: '#3730a3',
          900: '#312e81',
        },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
      },
      boxShadow: {
        'card': '0 1px 2px rgba(0, 0, 0, 0.05)',
      },
    },
  },
};
```

---

## Figma/Design Reference

The design follows these principles:
1. **Minimal chrome:** Let the content breathe
2. **Consistent spacing:** 4px grid system
3. **Subtle depth:** Light shadows, not flat
4. **Clear hierarchy:** Bold headings, muted descriptions
5. **Action-oriented:** Primary buttons stand out
6. **Trust signals:** Professional but friendly
