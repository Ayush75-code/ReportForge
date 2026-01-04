# API Specifications & Schemas

## 1. API OVERVIEW
**Base URL:** `https://reportforge.report/api`
**Version:** v1
**Content-Type:** `application/json`

### Authentication
All requests (except webhooks) require a Bearer Token from Supabase Auth.
`Authorization: Bearer <access_token>`

### Rate Limiting
*   **Trial:** 50 requests/hour, 5 reports/hour.
*   **Starter:** 100 requests/hour, 10 reports/hour.
*   **Pro:** 500 requests/hour, 50 reports/hour.
**Response Headers:**
*   `X-RateLimit-Limit`
*   `X-RateLimit-Remaining`
*   `X-RateLimit-Reset`

---

## 2. API ENDPOINTS

### Reports

#### **POST** `/api/reports/upload`
Uploads a CSV file, parses it, validates it, and creates a draft report.

**Request:** `multipart/form-data`
*   `file`: The CSV file (max 10MB).
*   `client_id`: UUID of the client.

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "report_id": "uuid",
    "status": "draft",
    "validation": {
      "rows_validated": 12,
      "total_spend": 2450.50,
      "campaigns": 12
    }
  }
}
```

**Errors:**
*   `400 Bad Request`: Validation Failed (Missing columns, empty file).

#### **POST** `/api/reports/:id/analyze`
Triggers the AI analysis job for a specific report.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "status": "completed",
    "analysis": {
      "summary": "Campaigns generated...",
      "wins": ["Win 1", "Win 2"],
      "concerns": ["Concern 1"],
      "recommendations": ["Rec 1"]
    }
  }
}
```

#### **POST** `/api/reports/:id/pdf`
Generates the PDF from the current analysis state.

**Body:** (Optional - overrides stored analysis)
```json
{
  "edited_analysis": {
    "summary": "User edited summary..."
  }
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "status": "completed",
    "pdf_url": "https://storage.supabase.co/..."
  }
}
```

### Clients

#### **GET** `/api/clients`
List all clients for the authenticated agency.

#### **POST** `/api/clients`
Create a new client.
**Body:** `{ "name": "Acme Corp", "website": "acme.com" }`

### Agencies

#### **PATCH** `/api/agencies/branding`
Update branding settings.
**Body:** `{ "logo_url": "...", "brand_color": "#4f46e5" }`

### Usage

#### **GET** `/api/usage`
Get current usage stats.
**Response:**
```json
{
  "period": "Jan 2024",
  "reports_used": 15,
  "reports_limit": 20,
  "plan": "starter"
}
```

---

## 3. DATA MODELS (JSON SCHEMAS)

### Report Object
```typescript
interface Report {
  id: string; // uuid
  client_id: string; // uuid
  agency_id: string; // uuid
  status: 'draft' | 'processing' | 'completed' | 'failed';
  created_at: string; // iso8601
  csv_path: string;
  pdf_url: string | null;
  metadata: {
    campaign_count: number;
    total_spend: number;
    date_range: { start: string, end: string };
  };
  ai_analysis: AIAnalysis | null;
}
```

### AIAnalysis Object
```typescript
interface AIAnalysis {
  summary: string;
  wins: string[]; // Max 3 items
  concerns: string[]; // Max 2 items
  recommendations: string[]; // Max 3 items
}
```

### Agency Object
```typescript
interface Agency {
  id: string;
  name: string;
  email: string;
  branding: {
    logo_url: string | null;
    brand_color: string; // default #6366f1
  };
  subscription: {
    plan: 'trial' | 'starter' | 'pro';
    status: 'active' | 'canceled' | 'past_due';
  };
}
```
