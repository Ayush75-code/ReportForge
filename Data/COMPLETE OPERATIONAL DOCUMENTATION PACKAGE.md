COMPLETE OPERATIONAL DOCUMENTATION PACKAGE
📋 TABLE OF CONTENTS
PROCEDURAL & OPERATIONAL FILES

Standard Operating Procedures (SOPs)
Escalation Matrix
Compliance & Policy Documents

KNOWLEDGE BASE FILES

Product Manual & Feature Specifications
Structured FAQ List
Troubleshooting Guide

INTERACTION & CONTEXTUAL FILES

Response Templates
Interaction Transcripts (Golden Examples)
Golden Dataset (Evaluation Files)

TECHNICAL SPECIFICATION FILES

API Documentation
Data Field Specifications
Tool Authorization Matrix


1. STANDARD OPERATING PROCEDURES (SOPs)
SOP-001: CSV Upload & Validation Process
Objective
Ensure all uploaded CSV files meet Meta Ads format requirements before consuming API resources.
Scope
Applies to all CSV uploads via /api/reports/upload endpoint.
Procedure
Step 1: Pre-Upload Validation (Client-Side)
1.1 Check file extension
    - ACCEPT: .csv only
    - REJECT: .xls, .xlsx, .txt with message:
      "Please upload a CSV file. Excel files must be saved as CSV first."

1.2 Check file size
    - ACCEPT: ≤ 10 MB
    - REJECT: > 10 MB with message:
      "File too large. Please contact support@insightengine.com for enterprise processing."

1.3 Display loading indicator
    - Show: "Uploading and validating..."
Step 2: Server-Side Validation
2.1 Parse CSV headers
    - Use PapaParse with config: { header: true, skipEmptyLines: true }
    - Timeout: 5 seconds
    - IF timeout → Return error: "File processing timeout. Please try again."

2.2 Validate required columns (exact match, case-sensitive)
    REQUIRED = [
      "Campaign Name",
      "Impressions", 
      "Clicks",
      "Spend",
      "Conversions",
      "CPC",
      "CTR",
      "CPA"
    ]
    
    missing_columns = REQUIRED - parsed_headers
    
    IF missing_columns exists:
      RETURN {
        error: "Missing required columns",
        missing: missing_columns,
        found: parsed_headers,
        fix: "Please ensure your CSV includes: [list missing columns]"
      }
      STOP PROCESSING

2.3 Validate data types for each row
    FOR each row:
      - Campaign Name: string, non-empty
      - Impressions: integer > 0
      - Clicks: integer ≥ 0, ≤ Impressions
      - Spend: float ≥ 0, max 2 decimals
      - Conversions: integer ≥ 0, ≤ Clicks
      - CPC: float > 0
      - CTR: float, 0 < CTR < 100
      - CPA: float > 0 OR null (if conversions = 0)
      
      IF validation fails on any field:
        RETURN {
          error: "Invalid data in row {row_number}",
          field: "{field_name}",
          value: "{invalid_value}",
          expected: "{data type + constraints}"
        }
        STOP PROCESSING

2.4 Business logic validation
    total_spend = SUM(Spend)
    total_impressions = SUM(Impressions)
    
    IF total_spend = 0:
      RETURN error: "No ad spend detected. Please upload campaigns with actual spending."
      STOP PROCESSING
    
    IF total_impressions < 1000:
      ADD WARNING: "Low impression volume (<1000). Insights may lack statistical significance."
      CONTINUE (don't stop)

2.5 Store validated data
    INSERT INTO reports (
      client_id,
      agency_id,
      csv_filename,
      parsed_data,
      status
    ) VALUES (
      {client_id},
      {current_user_agency_id},
      {file.name},
      {validated_csv_as_json},
      'draft'
    )
    
    RETURN {
      report_id: {uuid},
      status: "validated",
      row_count: {number},
      warnings: [{optional_warnings}]
    }
Step 3: User Confirmation
3.1 Display validation summary
    Show user:
    - ✓ {X} campaigns validated
    - ✓ Total spend: ${Y}
    - ✓ Date range: {detected_range}
    - ⚠ Warnings (if any)

3.2 Prompt user action
    - Button: "Generate Report" → Proceeds to SOP-002
    - Button: "Upload Different File" → Returns to upload screen
Completion Criteria

Report record exists in database with status='draft'
User sees validation success message
System ready to proceed to AI analysis


SOP-002: AI-Powered Report Analysis Process
Objective
Generate structured marketing insights from validated Meta Ads data using AI.
Scope
Applies to reports with status='draft' after successful CSV validation.
Procedure
Step 1: Initiate Analysis
1.1 User clicks "Generate Report"
    - Frontend calls: POST /api/reports/analyze
    - Payload: { report_id: {uuid} }

1.2 Check processing lock (idempotency)
    SELECT status FROM reports WHERE id = {report_id}
    
    IF status = 'processing':
      RETURN { error: "Report already being processed", status: 409 }
      STOP
    
    IF status = 'completed':
      RETURN { message: "Report already generated", pdf_url: {existing_url} }
      STOP

1.3 Set processing lock
    UPDATE reports 
    SET 
      status = 'processing',
      processing_started_at = NOW()
    WHERE id = {report_id}
    
    START 30-second timeout timer
Step 2: Prepare AI Prompt
2.1 Retrieve report data
    SELECT 
      r.parsed_data,
      c.client_name,
      a.agency_name
    FROM reports r
    JOIN clients c ON r.client_id = c.id
    JOIN agencies a ON r.agency_id = a.id
    WHERE r.id = {report_id}

2.2 Calculate aggregate metrics
    total_impressions = SUM(parsed_data[].Impressions)
    total_clicks = SUM(parsed_data[].Clicks)
    total_spend = SUM(parsed_data[].Spend)
    total_conversions = SUM(parsed_data[].Conversions)
    overall_ctr = (total_clicks / total_impressions) * 100
    overall_cpa = total_spend / total_conversions

2.3 Construct system prompt
    SYSTEM_PROMPT = "
    You are an expert Meta Ads analyst with 10+ years of experience.
    Analyze campaign performance data and provide actionable insights.
    
    CRITICAL: Respond ONLY with valid JSON matching this exact structure:
    {
      "summary": "2-3 sentence executive overview with specific metrics",
      "wins": [
        "Campaign-specific win with numbers",
        "Campaign-specific win with numbers",
        "Campaign-specific win with numbers"
      ],
      "concerns": [
        "Specific issue with campaign name and metric",
        "Specific issue with campaign name and metric"
      ],
      "recommendations": [
        "Actionable step with campaign name",
        "Actionable step with campaign name",
        "Actionable step with campaign name"
      ]
    }
    
    Rules:
    - Use specific campaign names
    - Include actual numbers from data
    - Be direct and actionable
    - No generic statements
    "
    
    USER_PROMPT = "
    Analyze this Meta Ads performance data for {client_name}:
    
    Aggregate Metrics:
    - Total Spend: ${total_spend}
    - Total Impressions: {total_impressions}
    - Total Clicks: {total_clicks}
    - Total Conversions: {total_conversions}
    - Overall CTR: {overall_ctr}%
    - Overall CPA: ${overall_cpa}
    
    Campaign Details:
    {JSON.stringify(parsed_data, null, 2)}
    "
Step 3: Call OpenAI API
3.1 Make API request
    response = await openai.chat.completions.create({
      model: "gpt-4o-mini",
      messages: [
        { role: "system", content: SYSTEM_PROMPT },
        { role: "user", content: USER_PROMPT }
      ],
      response_format: { type: "json_object" },
      temperature: 0.3,
      max_tokens: 1500,
      timeout: 25000 // 25 seconds
    })

3.2 Parse and validate response
    TRY:
      analysis = JSON.parse(response.choices[0].message.content)
    CATCH ParseError:
      LOG error: "OpenAI returned invalid JSON"
      GOTO Step 3.3 (retry)
    
    VALIDATE structure:
      - analysis.summary exists and is string
      - analysis.wins is array with exactly 3 items
      - analysis.concerns is array with exactly 2 items
      - analysis.recommendations is array with exactly 3 items
    
    IF validation fails:
      LOG error: "OpenAI response missing required fields"
      GOTO Step 3.3 (retry)

3.3 Retry logic (if needed)
    IF first attempt failed:
      WAIT 2 seconds
      RETRY with stricter prompt:
        Add to system prompt: "You MUST return valid JSON with all required fields."
      
      IF second attempt also fails:
        GOTO Step 4 (Escalation)
Step 4: Handle Success or Failure
4.1 On Success:
    UPDATE reports
    SET
      ai_analysis = {analysis_json},
      status = 'completed',
      completed_at = NOW()
    WHERE id = {report_id}
    
    PROCEED TO SOP-003 (PDF Generation)

4.2 On Failure (after retry):
    UPDATE reports
    SET
      status = 'failed',
      error_message = 'AI analysis failed - manual review required'
    WHERE id = {report_id}
    
    SEND escalation notification (see Escalation Matrix)
    
    RETURN to user:
    {
      error: "We're having trouble analyzing this data",
      message: "Our team has been notified and will email you within 2 hours",
      support_email: "support@insightengine.com"
    }
Completion Criteria

Report status updated to 'completed' or 'failed'
User notified of outcome
If failed: escalation ticket created


SOP-003: PDF Report Generation & Delivery
Objective
Convert AI-generated analysis into a professionally branded PDF document.
Scope
Applies to reports with status='completed' (after successful AI analysis).
Procedure
Step 1: Retrieve Report Data
1.1 Fetch complete report context
    SELECT
      r.id,
      r.ai_analysis,
      c.client_name,
      a.agency_name,
      a.logo_url,
      a.brand_color
    FROM reports r
    JOIN clients c ON r.client_id = c.id
    JOIN agencies a ON r.agency_id = a.id
    WHERE r.id = {report_id}

1.2 Validate brand assets
    IF logo_url is NOT NULL:
      CHECK logo_url accessibility (HTTP HEAD request)
      IF status != 200:
        LOG warning: "Logo URL inaccessible: {logo_url}"
        USE fallback: agency_name as text header
    
    IF brand_color is invalid hex:
      LOG warning: "Invalid brand color: {brand_color}"
      USE fallback: #6366f1 (default indigo)
Step 2: Compose PDF Document
2.1 Create React-PDF Document structure
    import { Document, Page, Text, View, Image } from '@react-pdf/renderer';
    
    const ReportDocument = (
      <Document>
        <Page size="A4" style={styles.page}>
          
          {/* HEADER */}
          <View style={styles.header}>
            {logo_url ? (
              <Image src={logo_url} style={styles.logo} />
            ) : (
              <Text style={styles.agencyName}>{agency_name}</Text>
            )}
            <View style={styles.headerRight}>
              <Text style={styles.reportTitle}>Meta Ads Performance Report</Text>
              <Text style={styles.clientName}>{client_name}</Text>
              <Text style={styles.date}>{formatted_date}</Text>
            </View>
          </View>
          
          {/* DIVIDER */}
          <View style={[styles.divider, { borderColor: brand_color }]} />
          
          {/* EXECUTIVE SUMMARY */}
          <View style={styles.section}>
            <Text style={styles.sectionHeader}>Executive Summary</Text>
            <View style={styles.summaryBox}>
              <Text style={styles.bodyText}>
                {ai_analysis.summary}
              </Text>
            </View>
          </View>
          
          {/* KEY WINS */}
          <View style={styles.section}>
            <Text style={styles.sectionHeader}>✓ Key Wins</Text>
            {ai_analysis.wins.map((win, i) => (
              <View style={styles.bulletItem} key={i}>
                <View style={styles.greenCheckmark}>
                  <Text style={styles.checkmarkIcon}>✓</Text>
                </View>
                <Text style={styles.bulletText}>{win}</Text>
              </View>
            ))}
          </View>
          
          {/* CONCERNS */}
          <View style={styles.section}>
            <Text style={styles.sectionHeader}>⚠ Areas of Concern</Text>
            {ai_analysis.concerns.map((concern, i) => (
              <View style={styles.bulletItem} key={i}>
                <View style={styles.amberWarning}>
                  <Text style={styles.warningIcon}>⚠</Text>
                </View>
                <Text style={styles.bulletText}>{concern}</Text>
              </View>
            ))}
          </View>
          
          {/* RECOMMENDATIONS */}
          <View style={styles.section}>
            <Text style={styles.sectionHeader}>→ Recommended Actions</Text>
            {ai_analysis.recommendations.map((rec, i) => (
              <View style={styles.bulletItem} key={i}>
                <View style={[styles.numberCircle, { backgroundColor: brand_color }]}>
                  <Text style={styles.numberText}>{i + 1}</Text>
                </View>
                <Text style={styles.bulletText}>{rec}</Text>
              </View>
            ))}
          </View>
          
          {/* FOOTER */}
          <View style={styles.footer}>
            <Text style={styles.footerText}>
              Generated by {agency_name} using InsightEngine
            </Text>
          </View>
          
        </Page>
      </Document>
    );

2.2 Apply styles (see Technical Specifications for full stylesheet)
Step 3: Generate PDF Buffer
3.1 Render to buffer
    TRY:
      const pdfBuffer = await pdf(ReportDocument).toBuffer();
    CATCH RenderError:
      LOG error: "PDF rendering failed: {error.message}"
      GOTO Step 5 (Error Handling)
    
    SET timeout: 10 seconds
    IF timeout exceeded:
      LOG error: "PDF generation timeout"
      GOTO Step 5 (Error Handling)

3.2 Validate output
    file_size_mb = pdfBuffer.length / (1024 * 1024)
    
    IF file_size_mb > 2:
      LOG warning: "PDF size exceeds 2MB: {file_size_mb}MB"
      // Note: This is acceptable but logged for monitoring
Step 4: Upload to Storage
4.1 Upload to Supabase Storage
    const fileName = `${report_id}.pdf`;
    
    TRY:
      const { data, error } = await supabase.storage
        .from('reports')
        .upload(fileName, pdfBuffer, {
          contentType: 'application/pdf',
          upsert: true
        });
      
      IF error:
        THROW StorageError
    
    CATCH StorageError:
      WAIT 2 seconds
      RETRY upload (max 3 attempts)
      
      IF all attempts fail:
        LOG error: "Storage upload failed after 3 attempts"
        GOTO Step 5 (Error Handling)

4.2 Generate public URL
    const { data: { publicUrl } } = supabase.storage
      .from('reports')
      .getPublicUrl(fileName);

4.3 Update database
    UPDATE reports
    SET pdf_url = {publicUrl}
    WHERE id = {report_id}
Step 5: Error Handling
5.1 On PDF generation failure:
    UPDATE reports
    SET status = 'pdf_generation_failed'
    WHERE id = {report_id}
    
    SEND escalation (see Escalation Matrix)
    
    RETURN to user:
    {
      status: "analysis_complete",
      message: "Your analysis is ready, but PDF generation is taking longer than expected",
      text_preview: {ai_analysis},
      note: "We'll email you the PDF within 30 minutes"
    }
    
    QUEUE background job: retry PDF generation every 5 minutes (max 3 retries)

5.2 On storage upload failure:
    Same as 5.1 above
Step 6: Delivery to User
6.1 Update UI
    RETURN to frontend:
    {
      status: "completed",
      pdf_url: {publicUrl},
      download_button: true,
      preview_available: true
    }

6.2 Track metrics
    INSERT INTO analytics_events (
      event_type: 'report_generated',
      report_id: {report_id},
      agency_id: {agency_id},
      generation_time_seconds: {elapsed_time}
    )

6.3 Increment usage counter
    UPDATE agencies
    SET reports_used_this_month = reports_used_this_month + 1
    WHERE id = {agency_id}
    
    Check if limit reached (see SOP-004)
Completion Criteria

PDF file uploaded to Supabase Storage
reports.pdf_url populated
User can download PDF
Usage counter updated


SOP-004: Usage Limit Enforcement
Objective
Prevent agencies from exceeding their monthly report generation limits.
Scope
Applies before initiating any report generation (checked in SOP-002 Step 1).
Procedure
Step 1: Check Current Usage
1.1 Query agency's usage
    SELECT 
      reports_used_this_month,
      subscription_status,
      plan_type
    FROM agencies
    WHERE id = {current_user_agency_id}

1.2 Determine limit based on plan
    plan_limits = {
      'starter': 20,
      'pro': 100,
      'enterprise': 999999 // effectively unlimited
    }
    
    current_limit = plan_limits[plan_type]
Step 2: Enforce Limit
2.1 Check if limit reached
    IF reports_used_this_month >= current_limit:
      
      IF subscription_status = 'trialing':
        RETURN {
          error: "Trial limit reached",
          used: reports_used_this_month,
          limit: current_limit,
          action: "upgrade_required",
          message: "You've used all {limit} reports in your trial. Upgrade to continue.",
          cta_button: "Upgrade to Pro",
          cta_url: "/settings/billing"
        }
        STOP PROCESSING
      
      ELSE IF subscription_status = 'active':
        RETURN {
          error: "Monthly limit reached",
          used: reports_used_this_month,
          limit: current_limit,
          reset_date: {first_day_of_next_month},
          message: "You've used all {limit} reports this month. Limit resets on {reset_date}.",
          options: [
            { label: "Upgrade Plan", url: "/settings/billing" },
            { label: "Wait until {reset_date}", action: "dismiss" }
          ]
        }
        STOP PROCESSING

2.2 If limit not reached
    ALLOW report generation to proceed
    // Usage counter will increment after successful generation (SOP-003 Step 6.3)
Step 3: Monthly Reset (Automated)
3.1 Scheduled job runs on 1st of each month at 00:00 UTC
    UPDATE agencies
    SET reports_used_this_month = 0
    WHERE TRUE

3.2 Log reset event
    INSERT INTO system_logs (
      event: 'monthly_usage_reset',
      affected_agencies: {count},
      timestamp: NOW()
    )
Completion Criteria

User either proceeds (if under limit) or sees upgrade prompt (if at limit)
No reports generated beyond limit
Usage resets correctly on 1st of month


SOP-005: Failed Report Recovery Process
Objective
Handle and recover reports that failed during AI analysis or PDF generation.
Scope
Applies to reports with status='failed' or 'pdf_generation_failed'.
Procedure
Step 1: Automated Retry (Background Job)
1.1 Job runs every 15 minutes
    SELECT id, status, error_message, created_at
    FROM reports
    WHERE status IN ('failed', 'pdf_generation_failed')
      AND created_at > NOW() - INTERVAL '24 hours'
      AND retry_count < 3

1.2 For each failed report:
    IF status = 'failed':
      // Retry AI analysis
      CALL SOP-002 (AI Analysis Process)
      
    ELSE IF status = 'pdf_generation_failed':
      // Retry PDF generation only
      CALL SOP-003 (PDF Generation Process)
    
    INCREMENT reports.retry_count

1.3 If retry succeeds:
    UPDATE reports SET status = 'completed'
    SEND email to user:
      "Good news! Your report for {client_name} is ready."
      [Download Button]
Step 2: Manual Review Escalation
2.1 If retry_count reaches 3:
    UPDATE reports SET status = 'requires_manual_review'
    
    CREATE support ticket:
    {
      type: 'failed_report_review',
      report_id: {uuid},
      agency_id: {uuid},
      error_summary: {error_message},
      csv_data_sample: {first_5_rows},
      priority: 'high'
    }

2.2 Notify support team via Slack:
    POST to #support-alerts channel:
    "🚨 Report {report_id} requires manual review
    Agency: {agency_name}
    Error: {error_message}
    [View in Admin Panel]"

2.3 Notify user via email:
    Subject: "We're working on your report"
    Body:
    "Hi {agency_name},
    
    We encountered an issue generating your report for {client_name}.
    Our team is reviewing it manually and will email you within 2 hours.
    
    If you have questions, reply to this email or contact support@insightengine.com.
    
    Reference ID: {report_id}"
Step 3: Manual Resolution
3.1 Support agent reviews report in admin panel
    - View CSV data
    - Check error logs
    - Identify root cause (data anomaly, API issue, etc.)

3.2 Agent takes action:
    OPTION A: Fix data and re-run
      - Manually correct any data issues
      - Trigger report regeneration
      - Mark ticket as resolved
    
    OPTION B: Contact agency for clarification
      - Send email requesting correct CSV export
      - Pause report processing
      - Document reason in ticket
    
    OPTION C: Escalate to engineering
      - If bug identified
      - Create GitHub issue
      - Temporarily disable affected feature if critical

3.3 Upon resolution:
    SEND email to user:
      "Your report is ready! [Download Link]"
      "Apology for the delay + brief explanation"
    
    CLOSE support ticket
    
    UPDATE reports SET
      status = 'completed',
      resolved_by = {support_agent_id},
      resolution_notes = {what_was_fixed}
Completion Criteria

Failed reports either auto-recover or escalate within 24 hours
Users notified at each stage
No report stuck in failed state indefinitely


2. ESCALATION MATRIX
When to Escalate: Decision Tree
┌─────────────────────────────────────────────────────────────┐
│                    ESCALATION DECISION TREE                  │
└─────────────────────────────────────────────────────────────┘

START: Issue Detected
    ↓
Is it a CRITICAL SYSTEM FAILURE?
    (Database down, authentication broken, payment processing failed)
    │
    ├─ YES → ESCALATE TO: Engineering On-Call
    │         Severity: P0 (Immediate)
    │         Contact: #engineering-oncall Slack + PagerDuty
    │         Response SLA: 15 minutes
    │
    └─ NO → Continue
           ↓
Is it a DATA SECURITY/PRIVACY issue?
    (Data leak, unauthorized access, GDPR violation)
    │
    ├─ YES → ESCALATE TO: Security Officer + CTO
    │         Severity: P0 (Immediate)
    │         Contact: security@insightengine.com + Direct phone
    │         Response SLA: 30 minutes
    │
    └─ NO → Continue
           ↓
Is it a BILLING/PAYMENT issue?
    (Charge failed, refund needed, subscription conflict)
    │
    ├─ YES → ESCALATE TO: Finance Team + Support Lead
    │         Severity: P1 (Urgent)
    │         Contact: #billing-support Slack
    │         Response SLA: 2 hours
    │
    └─ NO → Continue
           ↓
Has AUTOMATIC RETRY failed 3 times?
    (AI analysis failure, PDF generation failure)
    │
    ├─ YES → ESCALATE TO: Support Team (Manual Review)
    │         Severity: P2 (High)
    │         Contact: support queue + #support-alerts Slack
    │         Response SLA: 2 hours
    │
    └─ NO → Continue automatic retry process
           ↓
Is this a FEATURE REQUEST or FEEDBACK?
    │
    └─ YES → LOG TO: Product feedback database
              Severity: P3 (Low)
              No immediate escalation needed
              Weekly review by product team

Escalation Contacts & Channels
Issue TypeSeverityPrimary ContactSecondary ContactResponse SLASystem OutageP0#engineering-oncall (Slack)PagerDuty15 minData Breach/SecurityP0security@insightengine.comCTO direct phone30 minPayment FailureP1#billing-support (Slack)finance@insightengine.com2 hoursFailed Report (after 3 retries)P2Support queue ticket#support-alerts (Slack)2 hoursInvalid CSV FormatP3Auto-response to userSupport queue (if user replies)24 hoursFeature RequestP3feedback@insightengine.comProduct backlog1 week

Escalation Templates
Template 1: Failed Report Escalation (P2)
Slack Message to #support-alerts:
🔴 Report Generation Failed (3 retries exhausted)

Report ID: {report_id}
Agency: {agency_name} ({agency_email})
Client: {client_name}
Error: {error_message}
CSV Rows: {row_count}
First Failure: {timestamp}

[View in Admin Panel] [View CSV Data] [Contact Agency]
Email to Support Team:
Subject: [P2] Manual Review Needed - Report {report_id}

Team,

A report has failed after 3 automatic retry attempts and requires manual review.

Details:
- Agency: {agency_name}
- Client: {client_name}
- Error: {error_message}
- Report ID: {report_id}
- Admin Link: https://admin.insightengine.com/reports/{report_id}

Action Required:
1. Review CSV data for anomalies
2. Check error logs for root cause
3. Either:
   - Fix data and re-run, OR
   - Contact agency for correct file, OR
   - Escalate to engineering if bug detected

Target Resolution: 2 hours from now ({target_time})

Thanks,
InsightEngine Monitoring System

Template 2: Security Incident Escalation (P0)
Immediate Notification (Slack + Email + Phone):
🚨 SECURITY INCIDENT DETECTED 🚨

Incident ID: {incident_id}
Detected At: {timestamp}
Type: {unauthorized_access / data_exposure / suspicious_activity}

Details:
{brief_description}

Affected Systems: {list}
Affected Users: {count or "investigating"}

IMMEDIATE ACTIONS TAKEN:
- {auto_action_1}
- {auto_action_2}

REQUIRED NEXT STEPS:
1. {action_1}
2. {action_2}

Security Officer and CTO: Please acknowledge receipt within 15 minutes.

View Incident Details: {admin_panel_link}

Template 3: Billing Issue Escalation (P1)
Slack Message to #billing-support:
💳 Payment Processing Issue

User: {agency_name} ({email})
Issue: {charge_failed / refund_needed / subscription_conflict}
Amount: ${amount}
Stripe Customer ID: {stripe_customer_id}
Error: {stripe_error_message}

Context:
{brief_description}

[View in Stripe Dashboard] [View User Profile] [Contact User]

Escalation Procedures by Severity
P0 (Critical) - Immediate Response Required
Definition: System-wide outage, security breach, or issue affecting all users.
Response Protocol:

Automated Alert triggers within 60 seconds of detection
On-call engineer paged via PagerDuty (must acknowledge within 5 min)
Incident channel created in Slack (#incident-YYYY-MM-DD)
Status page updated: https://status.insightengine.com
All hands notification if outage exceeds 30 minutes

Resolution Requirements:

Incident commander assigned
Hourly updates to stakeholders
Post-mortem required within 48 hours


P1 (Urgent) - Same-Day Resolution
Definition: Issue affecting single user's ability to use paid features (billing, report access).
Response Protocol:

Support ticket created automatically
Slack alert to #billing-support or #support-alerts
Agent assigned within 30 minutes
User notified of escalation within 1 hour

Resolution Requirements:

Must be resolved or have clear workaround within 4 hours
User updated every 2 hours until resolved
Manager approval needed for refunds > $50


P2 (High) - Next Business Day
Definition: Report generation failure, feature not working for single user, data quality issue.
3 Jan
Response Protocol:

Automatic retry attempted 3 times over 30 minutes
Support ticket created after final retry failure
Slack notification to #support-alerts
Manual review assigned within 2 hours during business hours
Resolution Requirements:

Must be triaged within 2 hours (business hours)
User receives initial response within 2 hours
Resolution or workaround within 24 hours
Root cause documented in ticket
P3 (Low) - This Week
Definition: Feature requests, non-critical bugs, user feedback, documentation issues.

Response Protocol:

Logged automatically to product feedback database
Weekly review by product team
No immediate user notification unless they explicitly requested response
Resolution Requirements:

Reviewed in weekly product meeting
Added to backlog if accepted
User notified if feature is added to roadmap
Special Escalation Scenarios
Scenario 1: User Repeatedly Uploads Invalid CSVs
Detection: Same user triggers validation errors 5+ times in 1 hour

Action:

1. After 5th failure:
   - Display enhanced help modal with video tutorial
   - Offer "Schedule Demo Call" button
   
2. Create support ticket (P3)
   - Type: "User needs onboarding help"
   - Auto-assign to customer success team
   
3. Customer Success reaches out within 24 hours:
   "Hi {name}, noticed you're having trouble with CSV uploads.
   Would a quick 15-minute call help? [Calendar Link]"
No escalation to engineering unless:

User claims our validation is incorrect (escalate to P2)
Same issue affects 3+ different users (potential bug, escalate to P2)
Scenario 2: API Rate Limit Hit (OpenAI)
Detection: OpenAI API returns 429 status code

Action:

1. IMMEDIATE:
   - Queue report for retry in 60 seconds
   - DO NOT mark as failed yet
   
2. After 3 retries over 5 minutes:
   - Mark report as status='rate_limited'
   - Update user: "High demand - your report is queued and will generate within 30 minutes"
   
3. If backlog exceeds 20 reports:
   - ESCALATE TO: Engineering (P1)
   - Reason: May need to increase API quota or implement load balancing
   
4. Engineering assesses:
   - Is this temporary spike or sustained load?
   - Do we need to upgrade OpenAI tier?
   - Should we implement request queuing system?
User Communication:

Status: "Report in Progress"
Message: "Due to high demand, your report is queued. 
Expected completion: {estimated_time}
We'll email you when it's ready."

[OK] [Cancel Report]
Scenario 3: Suspicious Activity Detected
Detection Triggers:

10+ reports generated in under 1 minute
Same CSV uploaded 20+ times
API calls from unexpected IP addresses
Multiple failed authentication attempts
Action:

1. AUTOMATIC RESPONSE:
   - Temporarily rate-limit user (60 second cooldown)
   - Log suspicious activity with details
   
2. IF pattern continues (3+ rate-limits in 10 minutes):
   - Temporarily suspend account (5 minute cooldown)
   - ESCALATE TO: Security team (P1)
   - Send Slack alert: "🔐 Possible abuse detected - User {user_id}"
   
3. Security team investigates:
   - Review activity logs
   - Check if legitimate use case (e.g., batch processing)
   - Assess if account is compromised
   
4. Resolution options:
   - LEGITIMATE: Whitelist user, apologize, offer to discuss enterprise plan
   - ABUSE: Permanently ban account, refund if needed, document reason
   - COMPROMISED: Force password reset, notify user, investigate breach
User Communication (if legitimate):

Subject: "Account Temporarily Limited - Let's Chat"

Hi {name},

We noticed unusual activity on your account (20+ reports in 2 minutes) 
and temporarily applied a rate limit for security.

If you're processing reports in bulk, we'd love to help optimize your workflow!
Our enterprise plan includes:
- Higher rate limits
- Batch processing API
- Priority support

Reply to this email or schedule a call: [Calendar Link]

Best,
InsightEngine Security Team
3. COMPLIANCE & POLICY DOCUMENTS
Data Handling & Privacy Policy (GDPR/CCPA Compliance)
Scope
All data processed through InsightEngine, including:

Agency account information
Client names and business data
CSV uploads (Meta Ads performance data)
Generated reports and AI analysis
Core Principles
1. Data Minimization

We collect ONLY:
- Email address (for authentication)
- Agency name (for branding)
- Client names (for report headers)
- Marketing performance data (impressions, clicks, spend, conversions)

We DO NOT collect:
- Personal identifiable information (PII) of end consumers
- Credit card details (handled by Stripe, not stored by us)
- Social security numbers, addresses, phone numbers
- Sensitive personal data (health, religion, etc.)
2. Purpose Limitation

Data is used ONLY for:
- Generating performance reports
- Providing the core service
- Improving AI analysis quality (aggregate analysis only)
- Billing and account management

Data is NEVER used for:
- Selling to third parties
- Advertising to your clients
- Training public AI models
- Cross-agency analysis or benchmarking (without explicit consent)
3. Data Retention

ACTIVE DATA (while subscription active):
- Account info: Retained indefinitely
- Reports: Retained for 12 months, then auto-deleted
- CSV uploads: Deleted 30 days after report generation

AFTER ACCOUNT CANCELLATION:
- Grace period: 30 days (data accessible for export)
- After 30 days: All user data permanently deleted
- Exception: Billing records retained 7 years (legal requirement)

USER CAN REQUEST:
- Immediate deletion at any time (via support@insightengine.com)
- Data export in JSON format (processed within 48 hours)
4. Security Measures

- All data encrypted at rest (AES-256)
- All data encrypted in transit (TLS 1.3)
- Database access restricted to production services only
- No employee access to customer data without ticket justification
- Regular security audits and penetration testing
- SOC 2 Type II compliance (target: Q4 2024)
Acceptable Use Policy
Permitted Uses
✅ Allowed:

Generating reports for your own clients
Using reports in client presentations and proposals
Sharing reports with clients (they are white-labeled for you)
Uploading data from multiple advertising accounts
Creating reports for internal team review
✅ Encouraged:

Providing feedback on AI analysis accuracy
Requesting new features
Using reports to demonstrate value to clients
Prohibited Uses
❌ Not Allowed:

Reselling raw InsightEngine access to other agencies
Uploading data that is not yours (e.g., competitor data without permission)
Attempting to reverse-engineer or scrape our AI prompts
Using the service for illegal activities
Uploading CSVs containing personal information (email lists, names, etc.)
Sharing your account credentials with others
Creating multiple accounts to circumvent usage limits
❌ Abuse Detection:

We monitor for:
- Excessive API calls (>100 requests/hour without enterprise plan)
- Duplicate account creation from same payment method
- Patterns indicating credential sharing (logins from 5+ different IPs in 24 hours)

Consequences:
1st violation: Warning email + temporary rate limit
2nd violation: 7-day account suspension
3rd violation: Permanent ban + no refund
AI-Generated Content Disclaimer
Important Notice to Users
WHAT YOU MUST UNDERSTAND:

1. AI Analysis is NOT Perfect
   - Our AI provides insights based on patterns in data
   - It may occasionally misinterpret data or draw incorrect conclusions
   - YOU are responsible for reviewing reports before sending to clients

2. Your Expertise Still Matters
   - AI is a tool to save time, not replace your judgment
   - You should verify recommendations before implementing
   - If something seems wrong, trust your expertise

3. We Are Not Liable For
   - Business decisions made based on AI recommendations
   - Client losses if recommendations don't perform as expected
   - Errors in AI interpretation of unusual data patterns

4. Best Practices
   - Always review the generated summary for accuracy
   - Cross-reference with your own analysis for critical campaigns
   - Use AI insights as a starting point, not final answer
   - Add your own commentary when needed
Human-in-the-Loop Requirement
InsightEngine is designed with human oversight in mind:

✓ Reports are generated as DRAFTS
✓ You can edit analysis before downloading PDF
✓ You control when reports are shared with clients
✓ You own the final decision on all recommendations

We recommend:
- Spot-checking AI analysis against raw data
- Testing recommendations on small budgets first
- Maintaining your own strategic oversight
Refund & Cancellation Policy
Trial Period
- 14-day free trial (no credit card required)
- 10 free reports during trial
- Cancel anytime during trial with no charge
Paid Subscriptions
MONTHLY PLANS:
- Can cancel anytime
- Cancellation effective at end of current billing period
- Pro-rated refunds NOT available for partial months
- Access continues until end of paid period

ANNUAL PLANS:
- Can cancel anytime
- Pro-rated refund available if canceled within first 60 days
- After 60 days: No refund, but access continues until year-end
Refund Eligibility
ELIGIBLE FOR REFUND:
- Service completely unavailable for 48+ consecutive hours
- Critical feature not working for 7+ days despite support contact
- Billing error (charged wrong amount)
- Charged after cancellation

NOT ELIGIBLE:
- "Changed my mind" (after trial period)
- "Didn't use it enough"
- "Found a cheaper alternative"
- Reports generated successfully but "didn't like AI analysis quality"

PARTIAL REFUNDS:
We may offer partial refunds at our discretion for:
- Extended service degradation (>72 hours of issues)
- Feature advertised but not delivered
- Exceptional circumstances reviewed case-by-case
Refund Process
1. Email support@insightengine.com with:
   - Reason for refund request
   - Account email
   - Subscription date
   - Issue documentation (if technical problem)

2. Response within 2 business days

3. Approved refunds processed within 5-7 business days

4. Refunds issued to original payment method only
Content Ownership & Licensing
Who Owns What
YOU OWN:
- Your agency branding (logo, colors)
- Your client data
- The raw CSV files you upload
- The final PDF reports (after generation)

WE OWN:
- The InsightEngine platform and codebase
- The AI analysis methodology and prompts
- The PDF template designs (but you can use reports freely)

SHARED:
- AI-generated text analysis
  → We generate it, but you have full rights to use it
  → You can edit, modify, or rewrite it
  → You can claim it as your own work in client deliverables
What You Can Do With Reports
✅ Permitted:

Send to clients as your own work
Include in proposals and presentations
Post snippets on social media (with client permission)
Use in case studies (with client permission)
Edit or rewrite any AI-generated text
❌ Not Permitted:

Claim you built the InsightEngine platform
Resell report generation as a separate SaaS product
Remove "Generated by {Your Agency}" branding and claim it's someone else's
Third-Party Services & Sub-Processors
Services We Use (Your Data May Touch These)
Service	Purpose	Data Shared	Location	Privacy Policy
Supabase	Database & storage	All user data	US (AWS)	Link
OpenAI	AI analysis	Performance data only (no PII)	US	Link
Stripe	Payment processing	Email, payment info	US	Link
Vercel	Hosting	IP addresses, usage logs	US	Link
Postmark	Transactional emails	Email addresses	US	Link
Data Processing Agreements (DPA)
For GDPR compliance, we have DPAs in place with:
- Supabase (EU Standard Contractual Clauses)
- OpenAI (Business Terms with data handling provisions)
- Stripe (inherently compliant as payment processor)

If you need a copy of our DPAs for your own compliance:
Email: legal@insightengine.com
Response time: 5 business days
Incident Response Plan
What Qualifies as a Security Incident
TIER 1 - Critical:
- Unauthorized access to database
- Data breach (customer data exposed)
- Ransomware attack
- Complete service outage

TIER 2 - Serious:
- Suspicious login patterns affecting multiple accounts
- API key compromise
- DDoS attack (service degraded but not down)

TIER 3 - Minor:
- Single account compromise (weak password)
- Phishing email sent using our domain
- Minor vulnerability discovered
Response Timeline
TIER 1 (Critical):
T+0:      Automated detection triggers alert
T+15min:  On-call engineer acknowledges
T+30min:  Incident commander assigned, war room created
T+1hr:    Initial assessment complete, containment started
T+2hr:    Affected users notified (if data breach)
T+24hr:   Post-mortem draft completed
T+72hr:   Regulatory notifications filed (if required by law)

TIER 2 (Serious):
T+0:      Automated detection
T+1hr:    Engineering team reviews
T+4hr:    Containment plan executed
T+24hr:   Users notified if their accounts affected

TIER 3 (Minor):
T+0:      Ticket created
T+1day:   Reviewed and prioritized
T+1week:  Resolution deployed
User Notification Protocol
IF YOUR DATA WAS AFFECTED:
You will receive email within 2 hours containing:
- What happened (plain language explanation)
- What data was involved
- What we've done to contain it
- What you should do (e.g., change password)
- How to contact us for questions

IF YOUR DATA WAS NOT AFFECTED:
- We'll post update on status page
- Email notification to all users within 24 hours
- Details provided in monthly security newsletter
4. PRODUCT MANUAL & FEATURE SPECIFICATIONS
InsightEngine Product Overview
Product Name: InsightEngine
Version: 1.0 (MVP)
Target Users: Marketing agencies managing Meta Ads campaigns for clients
Core Value Proposition: Transform raw Meta Ads CSV exports into branded, AI-powered executive reports in under 60 seconds

Feature Catalog
FEATURE 1: CSV Upload & Validation
What It Does:

Accepts CSV files exported from Meta Ads Manager
Validates data structure and format
Provides clear error messages if file is invalid
How to Use:

1. Click "New Report" button
2. Select client from dropdown (or create new client)
3. Drag and drop CSV file OR click "Browse Files"
4. Wait for validation (typically 2-5 seconds)
5. If valid: See summary of data (campaigns, spend, etc.)
6. If invalid: See specific error message and fix instructions
Technical Specifications:

Supported File Types: .csv only
Maximum File Size: 10 MB
Maximum Campaigns: 1,000 per file
Required Columns:
Campaign Name (text)
Impressions (integer)
Clicks (integer)
Spend (decimal, USD)
Conversions (integer)
CPC (decimal)
CTR (percentage)
CPA (decimal)
Error Handling:

Missing columns → Specific list of what's missing
Invalid data types → Row number and field name shown
Empty file → "File is empty" message
File too large → "Please contact support for enterprise processing"
Limitations:

Only Meta Ads format supported in v1.0
CSV must use comma as delimiter (not semicolon or tab)
Currency must be in USD (no conversion yet)
Date columns are optional (not used in analysis)
FEATURE 2: AI-Powered Performance Analysis
What It Does:

Analyzes campaign performance data using GPT-4o-mini
Identifies top-performing campaigns
Flags areas of concern
Generates specific, actionable recommendations
How to Use:

1. After CSV validation, click "Generate Analysis"
2. Wait 15-30 seconds while AI processes data
3. Review generated analysis:
   - Executive Summary
   - 3 Key Wins
   - 2 Areas of Concern
   - 3 Recommended Actions
4. (Optional) Edit any section before generating PDF
5. Click "Generate PDF Report"
What the AI Considers:

CTR Benchmarking: Compares against 1.5-2.5% industry average
CPA Efficiency: Identifies campaigns with outlier costs
Budget Distribution: Flags concentrated spending
Conversion Performance: Highlights campaigns with 0 conversions despite spend
Trend Analysis: (If historical data available in future versions)
AI Output Structure:

json
{
  "summary": "2-3 sentence overview with key metrics",
  "wins": [
    "Specific campaign + metric + why it's good",
    "Another win with numbers",
    "Third win"
  ],
  "concerns": [
    "Specific issue + campaign + impact",
    "Another concern"
  ],
  "recommendations": [
    "Actionable step 1 with campaign name",
    "Actionable step 2",
    "Actionable step 3"
  ]
}
```

**Limitations:**
- AI cannot access external data (e.g., landing page quality, creative assets)
- Analysis is based solely on performance metrics provided
- AI may occasionally misinterpret unusual data patterns
- Recommendations are generic best practices, not client-specific strategy

**Best Practices:**
- Always review AI analysis before sending to clients
- Add your own context where AI lacks industry-specific knowledge
- Use AI insights as starting point, not final answer
- Test recommendations on small budgets first

---

### **FEATURE 3: Branded PDF Report Generation**

**What It Does:**
- Converts AI analysis into professional PDF document
- Applies your agency branding (logo + colors)
- Formats content for easy reading
- Generates download link

**How to Use:**
```
1. After reviewing analysis, click "Generate PDF"
2. Wait 5-10 seconds for PDF rendering
3. Preview PDF in browser OR download immediately
4. Share with client via email, Slack, or direct link
```

**Customization Options (in v1.0):**
- **Agency Logo:** Upload in Settings → Branding (PNG/JPG, max 500KB)
- **Brand Color:** Set hex code in Settings → Branding
- **Agency Name:** Used in report footer

**PDF Specifications:**
- **Page Size:** A4 (8.27" × 11.69")
- **File Format:** PDF 1.7
- **File Size:** Typically 200-500 KB
- **Fonts:** Helvetica (universally supported)
- **Color Space:** RGB
- **Resolution:** 150 DPI (print-ready)

**PDF Layout:**
```
┌─────────────────────────────────────┐
│  [Logo]     Meta Ads Performance     │
│             {Client Name}            │
│             {Date}                   │
├─────────────────────────────────────┤
│                                     │
│  Executive Summary                  │
│  [2-3 sentence overview]            │
│                                     │
│  ✓ Key Wins                         │
│  • Win 1 with data                  │
│  • Win 2 with data                  │
│  • Win 3 with data                  │
│                                     │
│  ⚠ Areas of Concern                 │
│  • Concern 1 with specifics         │
│  • Concern 2 with specifics         │
│                                     │
│  → Recommended Actions              │
│  1. Action 1 with campaign          │
│  2. Action 2 with campaign          │
│  3. Action 3 with campaign          │
│                                     │
├─────────────────────────────────────┤
│  Generated by {Agency} using        │
│  InsightEngine                      │
└─────────────────────────────────────┘
```

**Limitations:**
- No charts/graphs in v1.0 (text and numbers only)
- Single-page format only
- Cannot customize section order
- No multi-language support yet (English only)

**Accessibility:**
- PDF is screen-reader friendly
- Text is selectable and searchable
- WCAG AA compliant contrast ratios

---

### **FEATURE 4: Client Management**

**What It Does:**
- Organize reports by client
- Track which clients have recent reports
- Quick access to client history

**How to Use:**
```
1. Go to "Clients" page
2. Click "Add Client" to create new
3. Enter:
   - Client Name (required)
   - Website (optional)
   - Industry (optional, for future features)
4. Save client
5. When creating reports, select from client dropdown
```

**Client List View:**
```
┌────────────────────────────────────────────────────┐
│  Client Name    | Last Report  | Total Reports    │
├────────────────────────────────────────────────────┤
│  Acme Corp      | 2 days ago   | 12              │
│  Beta LLC       | 1 week ago   | 8               │
│  Gamma Inc      | 3 weeks ago  | 5               │
└────────────────────────────────────────────────────┘
```

**Client Detail View:**
```
Client: Acme Corp
Website: acmecorp.com
Industry: E-commerce

Recent Reports:
- Jan 15, 2024: Q4 Performance Report [Download]
- Dec 20, 2023: Holiday Campaign Analysis [Download]
- Nov 10, 2023: November Performance [Download]

[Generate New Report] [Edit Client] [Delete Client]
```

**Limitations:**
- Cannot merge duplicate clients (manual process)
- No client-level settings or customization yet
- Cannot bulk-upload clients (one at a time)

---

### **FEATURE 5: Report History & Management**

**What It Does:**
- View all past reports
- Re-download previously generated PDFs
- Track report generation dates
- Delete old reports

**How to Use:**
```
1. Go to "Reports" page
2. See list of all reports sorted by date
3. Filter by:
   - Client
   - Date range
   - Status (completed, failed, processing)
4. Actions available:
   - Download PDF
   - View analysis (text only)
   - Delete report
```

**Report List View:**
```
┌──────────────────────────────────────────────────────────────┐
│  Date       | Client      | Status     | Actions            │
├──────────────────────────────────────────────────────────────┤
│  Jan 15     | Acme Corp   | Completed  | [Download] [Delete]│
│  Jan 12     | Beta LLC    | Completed  | [Download] [Delete]│
│  Jan 10     | Gamma Inc   | Failed     | [Retry] [Delete]   │
│  Jan 08     | Acme Corp   | Completed  | [Download] [Delete]│
└──────────────────────────────────────────────────────────────┘
```

**Auto-Deletion Policy:**
- Reports older than 12 months are automatically deleted
- You'll receive email notification 30 days before deletion
- Download reports you want to keep long-term

**Storage Limits:**
- Starter Plan: 20 reports max stored at once
- Pro Plan: 100 reports max stored
- If limit reached: Delete old reports or upgrade plan

---

### **FEATURE 6: Usage Tracking & Limits**

**What It Does:**
- Shows how many reports you've generated this month
- Warns when approaching limit
- Prevents generation beyond plan limit

**How to View:**
```
Dashboard shows:
"Reports This Month: 15 / 20"
[Progress bar showing 75% used]

When approaching limit (18/20):
⚠️ Warning banner: "You have 2 reports remaining this month"

When limit reached (20/20):
🛑 "Monthly limit reached. Resets on Feb 1 or upgrade to Pro"
[Upgrade Button]
```

**Plan Limits:**
- **Trial:** 10 reports total
- **Starter ($99/mo):** 20 reports/month
- **Pro ($199/mo):** 100 reports/month
- **Enterprise (custom):** Unlimited

**Reset Schedule:**
- Usage resets on 1st of each month at 00:00 UTC
- Pro-rated if you upgrade mid-month

---

## **Keyboard Shortcuts**
```
GLOBAL:
Cmd/Ctrl + K     → Quick search (clients, reports)
Cmd/Ctrl + N     → New report
Cmd/Ctrl + ,     → Settings

REPORT CREATION:
Cmd/Ctrl + U     → Upload CSV
Cmd/Ctrl + Enter → Generate analysis (after validation)
Cmd/Ctrl + P     → Generate PDF (after analysis)
Cmd/Ctrl + D     → Download PDF

NAVIGATION:
G then D         → Go to Dashboard
G then C         → Go to Clients
G then R         → Go to Reports
G then S         → Go to Settings
Browser Support
Fully Supported:

Chrome 90+
Firefox 88+
Safari 14+
Edge 90+
Mobile Support:

iOS Safari 14+
Chrome Android 90+
Responsive design for tablets and phones
Not Supported:

Internet Explorer (any version)
Opera Mini
Browsers with JavaScript disabled
System Requirements
For Users:

Internet connection (minimum 1 Mbps)
Modern web browser (see above)
Screen resolution: minimum 1280×720
No software installation required
For CSV Exports:

Meta Ads Manager account
Permission to export campaign data
Data from at least 1 campaign with spend

5. STRUCTURED FAQ LIST
General Questions
Q1: What is InsightEngine?
A: InsightEngine is a tool that transforms raw Meta Ads performance data into professional, AI-powered executive reports. Upload a CSV from Meta Ads Manager, and get a branded PDF report with insights, wins, concerns, and recommendations in under 60 seconds.
Category: Product Overview
Keywords: what is, overview, description, purpose

Q2: Who is InsightEngine for?
A: InsightEngine is designed for marketing agencies and consultants who manage Meta Ads campaigns for multiple clients. It's perfect for account managers, media buyers, and agency owners who need to create professional client reports quickly.
Category: Product Overview
Keywords: target audience, who should use, ideal customer

Q3: How much does InsightEngine cost?
A:

Free Trial: 14 days, 10 reports (no credit card required)
Starter Plan: $99/month, 20 reports/month
Pro Plan: $199/month, 100 reports/month
Enterprise: Custom pricing for unlimited reports

All plans include AI analysis, branded PDFs, and unlimited clients.
Category: Pricing
Keywords: cost, price, plans, subscription

Q4: Do you offer annual billing?
A: Yes! Annual plans receive 2 months free:

Starter: $990/year (saves $198)
Pro: $1,990/year (saves $398)

You can switch from monthly to annual billing anytime in Settings → Billing.
Category: Pricing
Keywords: annual, yearly, discount, save money

Q5: Is there a free trial?
A: Yes! 14-day free trial with no credit card required. You get 10 free report generations to test the full platform. After the trial, choose a paid plan or your account will be paused (data is retained for 30 days).
Category: Trial & Onboarding
Keywords: free trial, demo, test, try before buy

Getting Started Questions
Q6: How do I get started?
A:

Sign up at insightengine.com (email + password)
Complete your agency profile (upload logo, set brand color)
Add your first client
Upload a Meta Ads CSV export
Click "Generate Report"
Download your branded PDF

Total time: 5-10 minutes for your first report.
Category: Getting Started
Keywords: how to start, setup, onboarding, first steps

Q7: Do I need to install any software?
A: No! InsightEngine is 100% web-based. Just log in through your browser (Chrome, Firefox, Safari, or Edge). Works on desktop, laptop, and tablet. No downloads, no plugins, no extensions required.
Category: Getting Started
Keywords: installation, software, download, setup

Q8: How do I export a CSV from Meta Ads Manager?
A:

Log into Meta Ads Manager (business.facebook.com)
Go to Campaigns tab
Select date range you want to report on
Click "Export" button (top right)
Choose "Export Table Data"
Select "CSV" format
Ensure these columns are visible before exporting:

Campaign Name, Impressions, Clicks, Spend, Conversions, CPC, CTR, CPA


Download the CSV file
Upload to InsightEngine

Category: CSV Export
Keywords: how to export, Meta Ads, Facebook Ads, CSV download

Q9: What if my CSV is missing required columns?
A: InsightEngine will show you exactly which columns are missing and how to fix it. Go back to Meta Ads Manager, click "Columns" dropdown, select "Customize Columns," and enable any missing metrics. Then re-export the CSV.
Common missing columns:

CPA (Cost Per Acquisition) - Enable under "Performance" metrics
Conversions - Enable under "Conversions" metrics

Category: CSV Export
Keywords: missing columns, CSV error, required fields

Report Generation Questions
Q10: How long does it take to generate a report?
A:

CSV Upload & Validation: 2-5 seconds
AI Analysis: 15-30 seconds
PDF Generation: 5-10 seconds
Total Time: 30-60 seconds from upload to download

If it takes longer than 2 minutes, check your internet connection or contact support.
Category: Report Generation
Keywords: how long, speed, time, duration

Q11: Can I edit the AI analysis before generating the PDF?
A: Yes! After AI generates the analysis, you can click "Edit" on any section (summary, wins, concerns, recommendations) to modify the text. Your changes will be reflected in the final PDF. This is useful for adding client-specific context or refining recommendations.
Category: Report Generation
Keywords: edit, customize, modify, change text

Q12: What if I don't like the AI's analysis?
A: You have several options:

Edit the text - Modify any section before generating PDF
Regenerate - Click "Try Again" to get different analysis
Manual override - Rewrite sections entirely with your own insights
Feedback - Report issues to us via the feedback button (we improve AI prompts based on your input)

Remember: AI is a tool to save time, not replace your expertise. Always review before sending to clients.
Category: Report Generation
Keywords: AI quality, bad analysis, incorrect insights, dissatisfied

Q13: Can I add charts or graphs to my reports?
A: Not in v1.0. The current version focuses on text-based executive summaries with specific metrics called out. Charts/graphs are on our roadmap for Q2 2024.
Workaround: Download the PDF and add charts manually in PowerPoint/Google Slides, or reference the original Meta Ads Manager charts in your client presentation alongside our report.
Category: Report Generation
Keywords: charts, graphs, visualizations, data viz

Q14: Can I generate reports for multiple campaigns at once?
A: Yes! Your CSV can include multiple campaigns (up to 1,000). The AI will analyze all campaigns together and identify:

Top performers (best campaigns)
Underperformers (campaigns needing attention)
Overall trends across all campaigns

Each report covers all campaigns in the uploaded CSV.
Category: Report Generation
Keywords: multiple campaigns, batch, bulk, many campaigns

Q15: What happens if my report generation fails?
A: InsightEngine automatically retries 3 times. If it still fails:

You'll see an error message with details
Our support team is automatically notified
We'll email you within 2 hours with either:

Your completed report, OR
Request for a corrected CSV file, OR
Explanation of the issue and next steps



Common causes of failures:

Corrupted CSV file (re-export and try again)
Extremely unusual data patterns (manual review needed)
Temporary API issues (automatic retry usually resolves)

Category: Troubleshooting
Keywords: failed report, error, not working, broken

Branding & Customization Questions
Q16: How do I add my agency logo?
A:

Go to Settings → Branding
Click "Upload Logo"
Select PNG or JPG file (max 500KB)
Logo should be square or horizontal (100×100 to 500×500 pixels recommended)
Save changes

Your logo will appear on all future reports. To update, just upload a new logo.
Category: Branding
Keywords: logo, branding, customization, white label

Q17: Can I change the brand color on reports?
A: Yes!

Go to Settings → Branding
Enter hex color code (e.g., #FF5733) or use the color picker
Preview how it looks
Save changes

The color is used for section headers, dividers, and recommendation numbers in your PDFs.
Category: Branding
Keywords: color, brand color, customization, theme

Q18: Can I remove "Generated by InsightEngine" from the footer?
A: Not in Starter or Pro plans. Enterprise plans can request footer customization. This is our way of keeping costs low for smaller agencies - the attribution helps us grow through word-of-mouth.
The footer reads: "Generated by {Your Agency Name} using InsightEngine"
Your agency name is prominent; our attribution is secondary.
Category: Branding
Keywords: footer, attribution, white label, remove branding

Q19: Can I create different report templates for different clients?
A: Not in v1.0. All reports use the same structure (summary, wins, concerns, recommendations). Custom templates are on our roadmap for Pro/Enterprise plans in Q3 2024.
Current workaround: Edit the AI-generated text to adjust tone/focus for different clients before generating PDF.
Category: Branding
Keywords: templates, customization, different formats, personalization

Account & Billing Questions
Q20: How do I cancel my subscription?
A:

Go to Settings → Billing
Click "Cancel Subscription"
Confirm cancellation
Your access continues until the end of your current billing period
After that, your account is paused (data retained for 30 days)

No cancellation fees. No questions asked.
Category: Billing
Keywords: cancel, cancellation, stop subscription, unsubscribe

Q21: What happens to my data if I cancel?
A:

First 30 days: All data is accessible (read-only). You can export or download past reports.
After 30 days: All data is permanently deleted (except billing records required by law)
Want to keep data longer? Downgrade to our free "Archive" plan ($0/month, no report generation, unlimited data storage) - coming Q2 2024

Category: Billing
Keywords: cancel, data deletion, what happens, account closure

Q22: Can I upgrade or downgrade my plan?
A: Yes, anytime!
Upgrading (Starter → Pro):

Takes effect immediately
You're charged pro-rated difference
Report limit increases immediately

Downgrading (Pro → Starter):

Takes effect at next billing cycle
You keep Pro features until then
No refund for unused portion of current month

To change plan: Settings → Billing → Change Plan
Category: Billing
Keywords: upgrade, downgrade, change plan, switch plan

Q23: What payment methods do you accept?
A: We accept all major credit and debit cards via Stripe:

Visa
Mastercard
American Express
Discover

We do NOT accept:

PayPal (coming Q2 2024)
Wire transfer (Enterprise only)
Cryptocurrency
Purchase orders (Enterprise only)

Category: Billing
Keywords: payment, credit card, how to pay, payment methods

Q24: Do you offer refunds?
A:
Yes, in these cases:

Service was down for 48+ hours
You were charged in error
Critical feature not working for 7+ days

No refunds for:

"Didn't use it enough" (trial is for testing)
"Changed my mind" (cancel before next billing cycle instead)
"Found cheaper alternative"

To request refund: Email support@insightengine.com with details. Response within 2 business days.
Category: Billing
Keywords: refund, money back, reimbursement, return

Q25: Is my payment information secure?
A: Yes. We use Stripe (industry-standard payment processor used by millions of businesses). We never see or store your credit card details - Stripe handles all payment data securely.
Security features:

PCI DSS compliant
3D Secure authentication
Encrypted transactions
Fraud detection

Category: Billing
Keywords: security, safe, payment security, credit card safety

Data & Privacy Questions
Q26: Is my client data secure?
A: Yes. Security measures include:

All data encrypted at rest (AES-256) and in transit (TLS 1.3)
Database access restricted to production services only
No employee access without justified support ticket
Regular security audits
SOC 2 Type II compliance (in progress)

Your data is never shared, sold, or used for training public AI models.
Category: Security & Privacy
Keywords: security, data protection, privacy, safe

Q27: Where is my data stored?
A: Primary data storage is in US-based AWS data centers (via Supabase). EU customers can request EU data residency for Enterprise plans (contact sales).
All storage locations are GDPR and CCPA compliant.
Category: Security & Privacy
Keywords: data location, where stored, data residency, GDPR

Q28: Do you use my data to train AI models?
A: No. Your data is used ONLY to:

Generate reports for you
Provide the core service
Improve our system (aggregate analysis only, no client-specific data)

We do NOT:

Sell your data
Use it to train public AI models
Share it with other agencies
Use it for marketing to your clients

Category: Security & Privacy
Keywords: data usage, AI training, privacy, data sharing

Q29: Can I export all my data?
A: Yes! Go to Settings → Data Export → Request Export. You'll receive a JSON file within 48 hours containing:

All client records
All report data (including AI analysis)
Account settings
Usage history

PDFs can be bulk-downloaded from the Reports page.
Category: Security & Privacy
Keywords: data export, download data, portability, GDPR

Q30: How do I delete my account and all data?
A:

Go to Settings → Account
Click "Delete Account"
Type your password to confirm
Choose: "Delete all data immediately" OR "Keep data for 30 days"
Confirm deletion

Warning: This is permanent and cannot be undone. Download any reports you want to keep first.
Category: Security & Privacy
Keywords: delete account, remove data, account deletion, GDPR right to erasure

Technical Questions
Q31: Which advertising platforms do you support?
A: Currently only Meta Ads (Facebook/Instagram) in v1.0.
Coming soon:

Google Ads (Q2 2024)
LinkedIn Ads (Q3 2024)
TikTok Ads (Q4 2024)

Want to request a platform? Email feedback@insightengine.com - we prioritize based on demand.
Category: Technical
Keywords: platforms, integrations, supported channels, Google Ads, LinkedIn

Q32: Can I integrate InsightEngine with my CRM or other tools?
A: Not yet. API access for integrations is coming in Q3 2024 for Pro and Enterprise plans.
Planned integrations:

Zapier (connect to 5,000+ apps)
Make.com (advanced automation)
Direct API (custom integrations)

Category: Technical
Keywords: API, integration, CRM, automation, Zapier

Q33: Do you have a mobile app?
A: No dedicated mobile app, but our website is fully responsive and works great on phones and tablets. You can upload CSVs, generate reports, and download PDFs from any mobile browser.
Native apps: Under consideration for 2025, depending on demand.
Category: Technical
Keywords: mobile app, iPhone, Android, app store

Q34: What browsers do you support?
A:
Fully supported:

Chrome 90+
Firefox 88+
Safari 14+
Edge 90+

Not supported:

Internet Explorer (any version)
Opera Mini
UC Browser

Category: Technical
Keywords: browser, compatibility, requirements, Chrome, Safari

Q35: Why can't I upload Excel files?
A: For security and consistency, we only accept CSV files. Excel files can contain macros, formulas, and multiple sheets that complicate parsing.
How to convert Excel to CSV:

Open your Excel file
File → Save As
Choose "CSV (Comma delimited)" format
Save and upload to InsightEngine

Category: Technical
Keywords: Excel, XLSX, file format, CSV conversion

Troubleshooting Questions
Q36: Why is my CSV failing validation?
A: Most common reasons:

Missing columns - Meta Ads export doesn't include all required fields

Fix: Customize columns in Meta Ads Manager before exporting


Wrong delimiter - File uses semicolons or tabs instead of commas

Fix: In Excel/Google Sheets, save as "CSV (Comma delimited)"


Extra header rows - Meta Ads sometimes adds summary rows at top

Fix: Delete any rows above the column headers in Excel


Special characters - Campaign names with emojis or unusual symbols

Fix: Acceptable in most cases, but try removing emojis if failing



Still not working? Email your CSV to support@insightengine.com and we'll diagnose it.
Category: Troubleshooting
Keywords: CSV error, validation failed, upload error, can't upload

Q37: The AI analysis seems generic or incorrect. What do I do?
A:
If it's too generic:

Your data might not have clear winners/losers (all campaigns performing similarly)
Try uploading more campaigns or longer date ranges for clearer patterns
Edit the analysis to add client-specific context

If it's factually incorrect:

Check if your CSV data is accurate (sometimes Meta Ads exports have glitches)
Click "Regenerate Analysis" to try again
Report the issue via the feedback button (include specifics so we can improve AI prompts)

Remember: AI provides a starting point. Your expertise should guide final recommendations.
Category: Troubleshooting
Keywords: AI wrong, bad analysis, generic insights, inaccurate

Q38: My logo looks blurry in the PDF. How do I fix it?
A:
Requirements for crisp logos:

Minimum resolution: 300×300 pixels
Recommended: 500×500 pixels or higher
Format: PNG (better quality than JPG)
Keep file size under 500KB

If your logo is blurry:

Re-export logo from your design tool at higher resolution
Use PNG instead of JPG
Ensure logo is actually high-res (zoom in on original file to check)

Category: Troubleshooting
Keywords: blurry logo, logo quality, image quality, pixelated

Q39: I forgot my password. How do I reset it?
A:

Go to insightengine.com/login
Click "Forgot Password?"
Enter your email address
Check email for password reset link (arrives in 1-2 minutes)
Click link and set new password

Didn't receive email?

Check spam/junk folder
Verify you entered correct email
Wait 5 minutes and try again
Still nothing? Email support@insightengine.com

Category: Troubleshooting
Keywords: forgot password, reset password, can't log in, locked out

Q40: The website is slow or not loading. What's wrong?
A:
Quick checks:

Check your internet connection
Try refreshing the page (Cmd/Ctrl + R)
Clear browser cache and cookies
Try a different browser
Check our status page: status.insightengine.com

If problem persists:

File a support ticket with details (browser, error message, what you were doing)
Check #status channel in our community Slack

Planned maintenance: We announce scheduled downtime 48 hours in advance via email and status page.
Category: Troubleshooting
Keywords: slow, not loading, error, website down, outage

Support Questions
Q41: How do I contact support?
A:
For urgent issues (account access, billing, service down):

Email: support@insightengine.com
Response time: 2 hours during business hours (Mon-Fri, 9am-5pm ET)

For general questions:

Help Center: help.insightengine.com
Community Slack: community.insightengine.com (peer support)

For feature requests:

Email: feedback@insightengine.com
Roadmap voting: roadmap.insightengine.com

For sales/enterprise inquiries:

Email: sales@insightengine.com

Category: Support
Keywords: contact, help, customer service, support email

Q42: What's your support response time?
A:
Starter Plan:

Email support: 2-4 hours (business hours)
No phone support

Pro Plan:

Priority email: 1-2 hours (business hours)
Live chat: Available
No phone support

Enterprise Plan:

Dedicated account manager
Phone support available
1-hour response time (24/7 for P0 issues)

Business hours: Monday-Friday, 9am-5pm Eastern Time (excluding US holidays)
Category: Support
Keywords: response time, SLA, how fast, support hours

Q43: Do you offer training or onboarding calls?
A:
Starter Plan:

Self-service onboarding (video tutorials, help docs)
No live training

Pro Plan:

One 30-minute onboarding call (optional)
Email success@insightengine.com to schedule

Enterprise Plan:

Dedicated onboarding (up to 2 hours)
Team training available
Custom training materials

For everyone: Comprehensive video tutorials at learn.insightengine.com
Category: Support
Keywords: training, onboarding, how to learn, tutorials, demo

Q44: Is there a community or user forum?
A: Yes! Join our Slack community at community.insightengine.com
What you'll find:

Peer support (other agency users)
Feature discussions and voting
Tips and best practices
Early access to beta features
Direct line to our product team

Rules: Be respectful, no spam, no competitor promotion.
Category: Support
Keywords: community, forum, Slack, user group, other users

Q45: Where can I see your product roadmap?
A: roadmap.insightengine.com
You can:

See what we're working on now
Vote on upcoming features
Submit feature requests
Track progress of requested features

We update the roadmap monthly and consider user votes heavily in prioritization.
Category: Product
Keywords: roadmap, upcoming features, future plans, what's next

6. TROUBLESHOOTING GUIDE
Problem Category: CSV Upload Issues

Issue 1: "Missing required columns" error
Symptoms:

Upload fails immediately
Error message lists specific missing columns
Can't proceed to analysis

Root Causes:

Meta Ads export doesn't include all metrics by default
Wrong report type selected in Meta Ads
Columns renamed manually in Excel

Diagnosis Steps:
1. Check error message - which columns are missing?
2. Common missing: "Conversions", "CPA", "CPC"
3. These are often hidden by default in Meta Ads
Solutions:
Solution A: Fix Meta Ads Export Settings
1. Go to Meta Ads Manager
2. Click "Columns" dropdown (above campaign list)
3. Select "Customize Columns"
4. Ensure these are checked:
   ☑ Campaign Name
   ☑ Impressions
   ☑ Link Clicks (or just "Clicks")
   ☑ Amount Spent (or "Spend")
   ☑ Conversions
   ☑ Cost Per Result (or "CPA")
   ☑ CTR (All)
   ☑ CPC (All)
5. Click "Apply"
6. Now export: "Export" → "Export Table Data" → CSV
7. Re-upload to InsightEngine
Solution B: Add Missing Columns in Excel (if minor)
ONLY if 1-2 columns missing and you know the values:

1. Open CSV in Excel/Google Sheets
2. Add column header exactly as specified in error
3. Enter values for each row
4. Save as CSV (comma delimited)
5. Re-upload

WARNING: Manual entry prone to errors. Prefer Solution A.
Prevention:

Save your Meta Ads column configuration as a preset
Use same preset for all exports
Document export process for your team

Escalation:
If columns are present but still showing as missing:

Email CSV to support@insightengine.com
Include screenshot of error
We'll diagnose within 2 hours


Issue 2: "Invalid data in row X" error
Symptoms:

CSV uploads successfully
Validation fails on specific row
Error shows row number and field name

Root Causes:

Corrupted export from Meta Ads
Manual edits introduced errors
Special characters in campaign names
Currency symbols in numeric fields

Diagnosis Steps:
1. Note the row number from error message
2. Open CSV in Excel/Google Sheets
3. Go to that row (add 1 to account for header row)
4. Check the flagged field for obvious issues:
   - Text in number fields
   - Negative values where not allowed
   - Missing values
   - Currency symbols ($, €, etc.)
Solutions:
Solution A: Fix Individual Row
1. Open CSV in spreadsheet app
2. Locate problem row
3. Common fixes:
   - Remove $ from Spend column (should be just numbers)
   - Fix negative Impressions/Clicks (should be 0 or positive)
   - Remove commas from large numbers (write 10000 not 10,000)
   - Replace blank cells with 0
4. Save as CSV
5. Re-upload
Solution B: Re-export from Meta Ads
If multiple rows have errors:
1. Don't try to fix manually
2. Go back to Meta Ads Manager
3. Export fresh CSV (ensure no filters applied)
4. Upload new file

Usually resolves corruption issues.
Solution C: Remove Problematic Campaigns
If one campaign consistently causes errors:
1. Delete that row from CSV
2. Generate report without it
3. Create separate report for that campaign later
4. Or contact support for help debugging
Prevention:

Export directly from Meta Ads (don't edit in Excel first)
Avoid copy-pasting from other sources
Don't add formulas or formatting to CSV files


Issue 3: File size too large (>10MB)
Symptoms:

Upload rejects file immediately
Error: "File exceeds 10MB limit"

Root Causes:

Exporting thousands of campaigns at once
Exporting years of historical data
Including extra columns not needed

Solutions:
Solution A: Reduce Date Range
Instead of exporting 12 months, try:
1. Export last 30 days only
2. Or last quarter (90 days)
3. Generate multiple reports for different periods

Benefits:
- Smaller file size
- More focused insights
- Faster processing
Solution B: Filter to Specific Campaigns
1. In Meta Ads Manager, apply campaign filter
2. Select campaigns for one client only
3. Export filtered view
4. Repeat for other clients

Don't export all clients' campaigns in one CSV.
Solution C: Remove Extra Columns
1. Before exporting, customize columns
2. Include ONLY the required 8 columns
3. Remove: Demographics, Placements, Devices, etc.
4. Smaller column count = smaller file
Solution D: Contact Support for Enterprise Processing
If you legitimately need to process >10MB files:
1. Email support@insightengine.com
2. We offer manual processing for large files
3. Or discuss Enterprise plan (higher limits)

Problem Category: Report Generation Issues

Issue 4: Report stuck in "Processing" status
Symptoms:

"Generating Analysis" spinner doesn't complete
Status stuck for >2 minutes
Can't proceed to PDF generation

Root Causes:

AI API timeout or rate limit
Unusually complex data pattern
Network connectivity issue
System overload (rare)

Diagnosis Steps:
1. Check how long it's been processing:
   - Under 2 minutes: Wait (normal for large files)
   - 2-5 minutes: Likely stuck
   - Over 5 minutes: Definitely stuck

2. Check your internet connection

3. Open browser dev console (F12) and look for errors
Solutions:
Solution A: Wait and Retry
1. Wait 5 full minutes (set a timer)
2. Refresh the page
3. Check Reports list - has status changed?
4. If status = "Failed", click "Retry"
5. If still "Processing", proceed to Solution B
Solution B: Manual Recovery
1. Go to Reports page
2. Find the stuck report
3. Click "Delete"
4. Upload CSV again and start fresh
5. Usually works on second attempt
Solution C: Simplify Data
If retry fails again:
1. Open your CSV
2. Remove campaigns with 0 spend
3. Remove campaigns with 0 impressions
4. Try uploading simplified version

Sometimes zero-value campaigns confuse AI.
Escalation:
If problem persists after 3 attempts:
1. DO NOT keep retrying (wastes your report quota)
2. Email support@insightengine.com with:
   - Report ID (shown in URL)
   - CSV file attached
   - Time you started generation
3. We'll process manually within 2 hours
Prevention:

Ensure stable internet connection
Don't close browser tab during generation
Avoid generating multiple reports simultaneously


Issue 5: AI analysis is too generic or unhelpful
Symptoms:

Analysis doesn't mention specific campaigns
Recommendations are vague ("Optimize targeting")
No concrete numbers in insights
Feels like template text

Root Causes:

Data doesn't have clear patterns (all campaigns similar)
Small data set (only 1-2 campaigns)
Very short date range (few days of data)
AI couldn't identify clear wins/losses

Diagnosis Steps:
1. Review your CSV:
   - How many campaigns? (Need 3+ for good analysis)
   - Date range? (Need 7+ days for significance)
   - Clear performance differences? (Some high CTR, some low?)

2. Check if all campaigns have similar metrics:
   - If all CPAs within $2 of each other, no clear "winner"
   - If all CTRs around 2%, hard to identify issues
Solutions:
Solution A: Add More Data
1. Export longer date range (30 days vs 7 days)
2. Include more campaigns (full account vs just 3)
3. Regenerate report

More data = clearer patterns = better insights.
Solution B: Manually Edit Analysis
1. Click "Edit" on any section of analysis
2. Add client-specific context AI doesn't know:
   - "Campaign X aligned with new product launch"
   - "Client requested focus on awareness this month"
   - "Campaign Y targeted competitor keywords"
3. Rewrite recommendations to be more specific
4. Generate PDF with your improved version
Solution C: Regenerate Analysis
1. Click "Try Again" button
2. AI will re-analyze with slightly different approach
3. Sometimes produces more specific insights on second try
4. Can regenerate up to 3 times per report
Solution D: Combine with Your Expertise
AI provides structure and time savings, but YOU add:
- Industry-specific knowledge
- Client history and context
- Strategic recommendations based on broader goals

Best reports = AI speed + Your expertise
When to Expect Generic Analysis (Normal):

All campaigns performing almost identically
Only 1-2 campaigns in account
Brand awareness campaigns (vanity metrics, not conversions)
Very low spend (<$100 total)

Escalation (continued):
If analysis is factually wrong (not just generic):

Click "Report Issue" button next to the analysis
Describe what's incorrect (e.g., "Says CPA decreased but it increased")
Attach CSV for our review
We'll investigate and improve AI prompts
You'll receive manually reviewed report within 4 hours


Issue 6: PDF generation fails
Symptoms:

AI analysis completes successfully
"Generate PDF" fails with error
Or PDF downloads but is corrupted/blank

Root Causes:

Logo image URL inaccessible
Brand color invalid
Special characters in text causing rendering issues
Temporary storage service issue

Diagnosis Steps:
1. Check error message:
   - "Logo failed to load" → Logo issue (see Solution A)
   - "PDF generation timeout" → System issue (see Solution B)
   - No specific error → Generic failure (see Solution C)

2. Try viewing analysis as text (not PDF):
   - Click "View Text Version" button
   - If text looks good, issue is PDF-specific
Solutions:
Solution A: Fix Logo Issue
1. Go to Settings → Branding
2. Check if logo preview loads
3. If not:
   - Re-upload logo
   - Ensure format is PNG or JPG (not SVG, WebP, etc.)
   - Ensure file size < 500KB
   - Ensure URL is publicly accessible (not behind login)
4. Save changes
5. Return to report and retry PDF generation
Solution B: Wait and Retry
If error is "timeout" or "storage error":
1. Wait 5 minutes
2. Click "Retry PDF Generation"
3. Usually resolves temporary system issues

If fails 3 times:
- Proceed to escalation (below)
Solution C: Simplify Text Content
If AI analysis has special characters causing issues:
1. Click "Edit" on analysis sections
2. Look for:
   - Emoji (remove them)
   - Smart quotes (" " vs " ")
   - Em dashes (— vs -)
   - Other unusual Unicode characters
3. Replace with simple ASCII equivalents
4. Save and retry PDF generation
Solution D: Use Text Version Temporarily
Workaround while waiting for fix:
1. Click "View Text Version"
2. Copy all text
3. Paste into Google Docs or Word
4. Add your logo manually
5. Export as PDF yourself

Not ideal, but gets report to client quickly.
Escalation:
If all solutions fail:
1. Report will be marked "PDF generation failed"
2. You'll automatically receive email when we fix it
3. PDF will be generated within 30 minutes (background job)
4. If still not working after 1 hour:
   - Email support@insightengine.com
   - Reference report ID
   - We'll manually generate and send to you

Problem Category: Account & Billing Issues

Issue 7: Payment declined
Symptoms:

Subscription renewal fails
Trial upgrade fails
Email notification about declined payment

Root Causes:

Card expired
Insufficient funds
Bank fraud prevention
Incorrect billing address

Diagnosis Steps:
1. Check email from Stripe for specific decline reason:
   - "Expired card" → Update card
   - "Insufficient funds" → Add funds or use different card
   - "Declined by bank" → Contact your bank
   - "Incorrect CVC" → Re-enter card details

2. Check card expiration date

3. Verify billing address matches card
Solutions:
Solution A: Update Payment Method
1. Go to Settings → Billing
2. Click "Update Payment Method"
3. Enter new card details
4. Ensure billing address is correct
5. Save changes
6. Subscription will auto-retry within 1 hour
Solution B: Contact Your Bank
If card is valid but keeps declining:
1. Call your bank/card issuer
2. Tell them: "Legitimate charge from InsightEngine via Stripe"
3. Ask them to allow the charge
4. Common issue: Bank fraud prevention blocking recurring charges
5. After bank approves, return to Settings → Billing → "Retry Payment"
Solution C: Use Different Card
If your card won't work:
1. Try different card (personal vs business, Visa vs Mastercard)
2. Company cards often have fewer restrictions
3. Virtual cards (Privacy.com, etc.) sometimes blocked by Stripe
Grace Period:
- 7 days to update payment before account is paused
- You'll receive reminders on Days 1, 3, 5, 7
- On Day 7, access is suspended (data retained)
- Update payment anytime within 30 days to restore access
- After 30 days, data is deleted
Escalation:
If payment issues persist after trying different cards:
1. Email support@insightengine.com
2. We can:
   - Generate manual invoice (pay via wire transfer - Enterprise only)
   - Investigate if there's a Stripe account issue
   - Offer payment plan for large upgrades

Issue 8: Charged incorrectly or duplicate charge
Symptoms:

Two charges on same day
Charged more than expected
Charged after canceling

Root Causes:

Browser double-click during checkout
Subscription AND usage overage charged together
Cancellation didn't process
Timezone confusion (charge appears day early/late)

Diagnosis Steps:
1. Check Settings → Billing → Transaction History
2. Look for:
   - Date and time of each charge
   - Amount and description
   - What triggered it (subscription vs overage)

3. Compare to your bank statement

4. Check cancellation confirmation email (if you canceled)
Solutions:
Solution A: Verify What You're Seeing
Common non-issues:
1. "Pending" charge + "Posted" charge = Same transaction shown twice
   - Check in 2-3 days; duplicate will disappear
   
2. Charged on different day than expected
   - Billing date is based on signup date, not calendar month
   - If signed up Jan 15, renewal is Feb 15 (not Feb 1)

3. Two separate charges on same day
   - One for subscription ($99)
   - One for overages if you exceeded limit
   - Both are legitimate
Solution B: Request Refund for Error
If genuinely charged in error:
1. Email support@insightengine.com with:
   - Transaction ID (from billing history)
   - Bank statement screenshot showing duplicate
   - Explanation of issue
2. We'll investigate within 2 business days
3. If confirmed error, refund processed within 5-7 days
Solution C: Cancel and Refund
If charged after canceling:
1. Forward cancellation confirmation email to support@insightengine.com
2. Include transaction ID of incorrect charge
3. We'll issue immediate refund
4. Usually happens if cancellation was <24 hours before billing date
Prevention:

Cancel at least 48 hours before next billing date
Check "Subscription Status" in Settings to confirm cancellation
Don't double-click payment buttons


Issue 9: Can't access account / "Account suspended"
Symptoms:

Login works but shows "Account Suspended" message
Can't generate reports
Limited access to dashboard

Root Causes:

Payment failure (exceeded grace period)
Usage limit exceeded and trial ended
Terms of service violation
Account flagged for suspicious activity

Diagnosis Steps:
1. Check suspension message for specific reason:
   - "Payment Required" → Billing issue
   - "Trial Ended" → Need to upgrade
   - "Suspicious Activity" → Security hold
   - "Terms Violation" → Policy issue

2. Check email for notifications we sent

3. Check Settings → Billing for payment status
Solutions:
Solution A: Resolve Payment (Most Common)
1. Go to Settings → Billing
2. Update payment method
3. Click "Resume Subscription"
4. Account reactivates within 5 minutes
5. All data still intact (nothing deleted)
Solution B: Upgrade from Trial
1. Click "Upgrade Now" button in suspension message
2. Choose plan (Starter or Pro)
3. Enter payment details
4. Account activated immediately
5. Previous trial reports still accessible
Solution C: Contact Support for Security Hold
If account was flagged for suspicious activity:
1. Email support@insightengine.com from your registered email
2. Explain your use case
3. Verify your identity (may ask security questions)
4. We'll review within 2 hours during business hours
5. If legitimate, account restored immediately
Solution D: Appeal Terms Violation
If suspended for policy violation:
1. Email support@insightengine.com
2. We'll explain specific violation
3. Options:
   - Acknowledge and agree to comply → Restore access
   - Dispute violation → Manual review (24-48 hours)
   - Accept termination → Data export provided

Common violations:
- Sharing account credentials
- Uploading non-advertising data
- Excessive API abuse
- Creating multiple accounts to bypass limits
Data Safety:

Data retained for 30 days during suspension
Can export data even while suspended
Restoration preserves all clients, reports, settings


Problem Category: Performance & Speed Issues

Issue 10: Website loading slowly
Symptoms:

Pages take >10 seconds to load
Buttons unresponsive
Upload progress bar stuck

Root Causes:

Poor internet connection
Browser cache/cookies overloaded
Too many browser tabs open
System-wide slowness (rare)

Diagnosis Steps:
1. Test internet speed: fast.com
   - Need minimum 1 Mbps
   - If < 1 Mbps, that's your issue

2. Check other websites
   - If they're also slow, it's your connection
   - If just InsightEngine is slow, proceed to solutions

3. Check status page: status.insightengine.com
   - If showing "Degraded Performance", system-wide issue
   - Otherwise, local issue
Solutions:
Solution A: Clear Browser Cache
Chrome:
1. Press Cmd/Ctrl + Shift + Delete
2. Select "All Time"
3. Check: Cached images and files, Cookies
4. Click "Clear Data"
5. Refresh InsightEngine page

Safari:
1. Safari → Preferences → Privacy
2. Click "Manage Website Data"
3. Find insightengine.com and Remove
4. Close and reopen browser

Firefox:
1. Menu → Options → Privacy & Security
2. Cookies and Site Data → Clear Data
3. Refresh page
Solution B: Try Different Browser
Test in another browser:
- If faster, original browser has issue → Clear cache or reinstall
- If same speed, likely internet or system issue
Solution C: Close Unnecessary Tabs
1. Close all tabs except InsightEngine
2. Close other applications
3. Restart browser
4. Try again

Browser performance degrades with 20+ tabs open.
Solution D: Restart Router
If internet speed test shows slow connection:
1. Unplug router for 30 seconds
2. Plug back in
3. Wait 2 minutes for full restart
4. Reconnect devices
5. Test speed again
Solution E: Check System Resources
If computer itself is slow:
1. Close memory-heavy applications
2. Restart computer
3. Check for system updates
4. Free up disk space (need 10%+ free)
Escalation:
If only InsightEngine is slow AND status page shows "All Systems Operational":
1. Email support@insightengine.com with:
   - Browser and version
   - Operating system
   - Internet speed test results
   - Screenshot of browser dev console errors (F12 → Console tab)
2. We'll investigate server-side issues

Issue 11: CSV upload progress stuck at 0% or 99%
Symptoms:

Upload starts but never completes
Progress bar frozen
No error message, just stuck

Root Causes:

Network timeout
File size close to limit
Corrupted file
Browser upload restrictions

Diagnosis Steps:
1. Check file size
   - If > 8MB, likely timeout issue
   - If < 1MB, likely corruption

2. How long have you waited?
   - < 2 minutes: Normal for large files, keep waiting
   - > 5 minutes: Definitely stuck

3. Check browser console for errors:
   - Press F12
   - Look for red error messages
Solutions:
Solution A: Cancel and Retry
1. Click "Cancel Upload" or close the modal
2. Wait 30 seconds
3. Try uploading again
4. Often works on second attempt
Solution B: Reduce File Size
1. Open CSV in Excel/Google Sheets
2. Remove unnecessary rows/columns
3. Save as new CSV
4. Upload the smaller version

Target: Under 5MB for reliable uploads
Solution C: Use Different Browser
Try uploading in:
1. Chrome (best compatibility)
2. Firefox (second choice)
3. Edge (third choice)

Avoid: Safari (known upload issues), Internet Explorer
Solution D: Check Internet Stability
1. Test: ping google.com (in terminal/command prompt)
2. Look for packet loss
3. If connection dropping:
   - Move closer to router
   - Use wired connection instead of WiFi
   - Pause other downloads/streams
Solution E: Split into Multiple Files
If file is very large:
1. Open in Excel
2. Split campaigns into groups (e.g., 50 campaigns per file)
3. Save as multiple CSVs
4. Generate separate reports
5. Combine insights manually
Escalation:
If problem persists:
1. Email CSV file to support@insightengine.com
2. We'll upload it manually on our end
3. You'll receive report within 2 hours
4. We'll investigate upload issue for future

Problem Category: Integration & Export Issues

Issue 12: Can't download PDF
Symptoms:

"Download PDF" button doesn't work
PDF downloads but is corrupted
PDF is blank when opened

Root Causes:

Browser blocking download
PDF not fully generated yet
File corrupted during generation
Storage service temporary issue

Diagnosis Steps:
1. Check browser's download bar
   - Is it showing "Download blocked"?
   - Is it still downloading (slow connection)?

2. Check browser download settings
   - Go to browser Settings → Downloads
   - Is "Ask where to save files" enabled?

3. Try opening PDF in different viewer
   - Adobe Acrobat
   - Browser built-in viewer
   - Preview (Mac) or Edge (Windows)
Solutions:
Solution A: Allow Pop-ups and Downloads
Chrome:
1. Click padlock icon in address bar
2. Check "Pop-ups and redirects" → Allowed
3. Check "Downloads" → Allowed
4. Refresh page and try again

Safari:
1. Safari → Preferences → Websites
2. Pop-up Windows → Find insightengine.com → Allow

Firefox:
1. Click shield icon in address bar
2. Disable "Enhanced Tracking Protection" for this site
Solution B: Try Direct Link
Instead of clicking button:
1. Right-click "Download PDF"
2. Select "Save Link As..."
3. Choose location and save
4. Open manually from file system
Solution C: Try Different Browser
Download issues are often browser-specific:
1. Copy report URL
2. Open in Chrome (most reliable for downloads)
3. Try downloading there
Solution D: Re-generate PDF
If PDF is corrupt:
1. Go to report page
2. Click "Regenerate PDF"
3. Wait for completion
4. Try downloading new version
Solution E: Email PDF to Yourself
Workaround:
1. Click "Email Report" button (if available - coming in future release)
2. Or contact support: "Please email report {report_id} to me"
3. We'll send PDF directly to your registered email
Prevention:

Update browser to latest version
Disable overly aggressive ad blockers on InsightEngine
Check available disk space (need 5-10MB free)


7. RESPONSE TEMPLATES
Email Templates
Template 1: Welcome Email (New User)
Subject: Welcome to InsightEngine! Here's how to get started
Body:
Hi {{first_name}},

Welcome to InsightEngine! 🎉

You've signed up for your 14-day free trial, which includes 10 free report generations.

HERE'S HOW TO CREATE YOUR FIRST REPORT (takes 5 minutes):

1. Add Your Branding
   → Go to Settings and upload your logo
   → Set your agency's brand color

2. Add a Client
   → Click "Clients" → "Add Client"
   → Enter your client's name

3. Export Data from Meta Ads
   → Log into Meta Ads Manager
   → Go to Campaigns
   → Click "Export" → "Export Table Data" → CSV
   → Make sure these columns are included:
     Campaign Name, Impressions, Clicks, Spend, Conversions, CPC, CTR, CPA

4. Generate Your First Report
   → Click "New Report"
   → Select your client
   → Upload the CSV file
   → Click "Generate Analysis"
   → Download your branded PDF

NEED HELP?
- Video tutorial: {{video_link}}
- Help docs: help.insightengine.com
- Reply to this email with questions

Your trial includes:
✓ 10 free reports
✓ All features unlocked
✓ No credit card required
✓ 14 days to test everything

Let's create something great together!

Best,
{{sender_name}}
InsightEngine Team

P.S. Have feedback? We read every reply and love hearing from users.

Template 2: Trial Ending Soon (Day 11 of 14)
Subject: Your InsightEngine trial ends in 3 days - Ready to upgrade?
Body:
Hi {{first_name}},

Your 14-day trial ends on {{expiration_date}} (3 days from now).

YOUR TRIAL STATS:
📊 Reports generated: {{reports_used}} / 10
👥 Clients added: {{client_count}}
⏱️ Total time saved: ~{{hours_saved}} hours

WHAT HAPPENS WHEN TRIAL ENDS?
- Your account will be paused
- You can still view past reports (read-only)
- To generate new reports, upgrade to a paid plan

CHOOSE YOUR PLAN:

Starter Plan - $99/month
- 20 reports per month
- Perfect for agencies with 5-10 clients
- All features included
[Upgrade to Starter →]

Pro Plan - $199/month
- 100 reports per month
- Priority support + live chat
- Best for agencies with 20+ clients
[Upgrade to Pro →]

QUESTIONS?
Reply to this email or schedule a call: {{calendar_link}}

We're here to help you decide!

Best,
{{sender_name}}
InsightEngine Team

P.S. Save 2 months with annual billing - just choose "Annual" at checkout.

Template 3: Payment Failed
Subject: ⚠️ Payment issue - Update your payment method
Body:
Hi {{first_name}},

We tried to charge your card ending in {{last_4}} for your InsightEngine subscription, but the payment was declined.

REASON: {{decline_reason}}

WHAT TO DO:
1. Go to Settings → Billing
2. Click "Update Payment Method"
3. Enter new card details
4. Your subscription will resume automatically

YOU HAVE 7 DAYS before your account is paused.

NEED HELP?
- Card won't work? Try a different card
- Bank blocking charge? Contact your bank and tell them to allow "InsightEngine via Stripe"
- Other issues? Reply to this email

YOUR DATA IS SAFE:
- All clients and reports are still saved
- Nothing will be deleted for 30 days
- Update payment anytime to restore full access

Update Payment Method: {{billing_url}}

Thanks,
{{sender_name}}
InsightEngine Team

P.S. If you meant to cancel, you can do that in Settings → Billing → Cancel Subscription

Template 4: Report Generation Failed (User Notification)
Subject: Report for {{client_name}} - We're working on it
Body:
Hi {{first_name}},

We encountered an issue generating your report for {{client_name}} (Report ID: {{report_id}}).

WHAT HAPPENED:
{{error_description}}

WHAT WE'RE DOING:
Our system is automatically retrying the generation. If that doesn't work, our support team has been notified and will manually review your report.

NEXT STEPS:
- We'll email you as soon as your report is ready (usually within 30 minutes)
- In the meantime, you can:
  • Try uploading a different CSV if you have one
  • Contact us if this is urgent: support@insightengine.com

Your report quota has NOT been used for this failed attempt.

Sorry for the inconvenience!

Best,
InsightEngine Monitoring System

---
Need immediate help? Reply to this email or call us at {{support_phone}} (Pro/Enterprise customers only).

Template 5: Report Ready After Manual Fix
Subject: ✅ Your report for {{client_name}} is ready!
Body:
Hi {{first_name}},

Good news! Your report for {{client_name}} has been generated and is ready to download.

We had to manually process this one due to {{issue_explanation}}, but everything is working now.

DOWNLOAD YOUR REPORT:
{{download_link}}

The report includes:
✓ Executive summary with key metrics
✓ 3 top-performing campaigns
✓ 2 areas of concern to address
✓ 3 actionable recommendations

PREVENTING THIS IN THE FUTURE:
{{prevention_tip}}

Thanks for your patience!

Best,
{{agent_name}}
InsightEngine Support Team

---
Questions about this report? Reply to this email - I'm here to help.

In-App Message Templates
Template 6: First Report Success (Celebration Modal)
🎉 Congratulations!

You just generated your first InsightEngine report!

WHAT'S NEXT?
- Share this report with your client
- Generate reports for other clients
- Customize your branding in Settings

[View Report] [Create Another]

Tip: Save time by adding all your clients now so they're ready for next time.

Template 7: Usage Limit Warning (80% Used)
⚠️ Usage Alert

You've used 16 of 20 reports this month (80%).

RESETS ON: {{reset_date}}

OPTIONS:
- Upgrade to Pro (100 reports/month) [Upgrade]
- Wait until {{reset_date}} for reset
- Prioritize remaining reports for high-value clients

[View Usage Details]

Template 8: Validation Error (Inline)
❌ CSV Validation Failed

Missing required columns:
- Conversions
- CPA

HOW TO FIX:
1. Go to Meta Ads Manager
2. Click "Columns" → "Customize Columns"
3. Enable "Conversions" and "Cost Per Result"
4. Re-export CSV and upload here

[Watch Tutorial Video] [Contact Support]

Support Response Templates
Template 9: Acknowledging Support Ticket
Subject: Re: {{ticket_subject}}
Body:
Hi {{user_name}},

Thanks for reaching out! I've received your message about {{issue_summary}}.

TICKET ID: #{{ticket_id}}
PRIORITY: {{priority_level}}
EXPECTED RESPONSE: Within {{sla_time}}

I'm looking into this now and will get back to you shortly with either a solution or follow-up questions.

In the meantime, you can:
- Check the status of this ticket: {{ticket_url}}
- View our help docs: help.insightengine.com
- Reply to this email with additional info

Thanks for your patience!

Best,
{{agent_name}}
InsightEngine Support

---
Ticket Reference: #{{ticket_id}}

Template 10: Issue Resolved
Subject: ✅ Resolved: {{ticket_subject}}
Body:
Hi {{user_name}},

Great news! I've resolved the issue with {{issue_description}}.

WHAT WAS FIXED:
{{solution_explanation}}

WHAT YOU NEED TO DO:
{{action_required_or_none}}

TEST IT:
Please try {{action_to_test}} and let me know if everything is working correctly now.

TO PREVENT THIS IN THE FUTURE:
{{prevention_tips}}

IS THIS RESOLVED?
- Yes, it's working! → No need to reply, we'll close this ticket
- No, still having issues → Reply to this email and I'll reopen

Thanks for reporting this - it helps us improve InsightEngine for everyone!

Best,
{{agent_name}}
InsightEngine Support

---
Ticket Reference: #{{ticket_id}}
Resolved at: {{timestamp}}

8. INTERACTION TRANSCRIPTS (Golden Examples)
Example 1: Successful First-Time User Flow
Context: New user, just signed up, trying to create first report.

USER: I just signed up. How do I create a report?
AGENT: Welcome to InsightEngine! I'll walk you through creating your first report - it takes about 5 minutes.
First, let's add your branding so reports look professional:

Go to Settings (gear icon, top right)
Upload your agency logo under "Branding"
Set your brand color (or use the default)

Done with that?

USER: Yep, logo is uploaded!
AGENT: Perfect! Now let's add a client:

Click "Clients" in the sidebar
Click "Add Client"
Enter the client's name (you can add website/industry too, but it's optional)
Save

Let me know when you've added your first client.

USER: Added "Acme Corp" as my client.
AGENT: Great! Now for the fun part - uploading data. You'll need a CSV export from Meta Ads Manager. Do you have one ready, or do you need instructions on how to export it?

USER: I need to export it. How do I do that?
AGENT: No problem! Here's exactly how:
In Meta Ads Manager:

Go to business.facebook.com → Select your Ad Account
Click the "Campaigns" tab
Set your date range (last 30 days is good for a monthly report)
Click the "Export" button (top right, looks like a download icon)
Choose "Export Table Data"
Select "CSV" as the format
Click "Export"

Important: Before exporting, make sure these columns are visible:

Campaign Name
Impressions
Clicks
Amount Spent
Conversions
CPC
CTR
Cost Per Result

If any are missing, click "Columns" → "Customize Columns" and add them.
Once you have the CSV, come back here and let me know!

USER: Got it! Downloaded the CSV. What's next?
AGENT: Perfect! Now:

Click "New Report" (big blue button on dashboard)
Select "Acme Corp" from the client dropdown
Drag and drop your CSV file, or click "Browse Files"
Wait a few seconds for validation

If the file is valid, you'll see a summary of your data. Then just click "Generate Analysis" and you're done!
Let me know if you hit any errors.

USER: It's validating... okay, it says "12 campaigns validated, $2,450 total spend". Looks good!
AGENT: Excellent! That means your CSV is formatted correctly. Now click the "Generate Analysis" button.
The AI will analyze your campaigns and create insights - this takes about 20-30 seconds. You'll see:

Executive summary
3 key wins
2 concerns to address
3 recommended actions

Once that's done, you can click "Generate PDF" to download your branded report.

USER: Wow, that was fast! The analysis looks really good. Downloading the PDF now... got it! This is perfect.
AGENT: Fantastic! You just created your first InsightEngine report! 🎉
Quick tips for next time:

You can edit the AI analysis before generating the PDF if you want to add your own insights
All your reports are saved in the "Reports" tab for future reference
You have 8 more free reports in your trial

Need anything else, or are you all set?

USER: All set! This is way easier than I thought. Thanks!
AGENT: My pleasure! Enjoy your trial, and feel free to reach out if you have any questions.
Happy reporting! 📊

TAKEAWAYS FROM THIS INTERACTION:
✅ Agent broke process into small, clear steps
✅ Checked for completion after each step
✅ Anticipated user's next question (how to export CSV)
✅ Provided exact instructions with visual cues ("looks like a download icon")
✅ Celebrated success and offered next steps
✅ Warm, friendly tone without being overly casual

Example 2: Handling CSV Validation Error
Context: User uploaded CSV but getting validation error.

USER: I'm trying to upload my CSV but it keeps saying "Missing required columns: Conversions, CPA". But I exported it from Meta Ads Manager like it says in your docs!
AGENT: I understand the frustration - this is actually a common issue! Meta Ads Manager doesn't include all columns by default, so you have to manually enable them before exporting.
Let me help you fix this quickly:
Step 1: Check your current columns
In Meta Ads Manager, look at the columns showing in your campaign view. Do you see "Conversions" and "Cost Per Result" (which is CPA)?

USER: I see "Results" but not "Conversions" or "Cost Per Result".
AGENT: Perfect - that's exactly the issue! "Results" is different from "Conversions". Here's how to add the correct columns:
In Meta Ads Manager:

Click the "Columns" dropdown (above your campaign list)
Select "Customize Columns"
In the search box, type "Conversions"
Check the box next to "Conversions" (under the "Performance" section)
Search for "Cost Per Result" and check that box too
Click "Apply"

Now you should see those columns in your view. THEN export the CSV again (Export → Export Table Data → CSV).
Try that and let me know if the new CSV uploads successfully!

USER: Okay, I added those columns and re-exported. Uploading now... it worked! It says "8 campaigns validated".
AGENT: Excellent! Glad that fixed it.
Pro tip for future exports: Once you've customized your columns, Meta Ads Manager should remember them. But if you ever switch between ad accounts, you may need to customize again.
You can also save this as a "Column Preset" in Meta Ads to make it even faster next time:

Customize Columns → (make your changes) → "Save as Preset" → Name it "InsightEngine Export"

Now you can proceed with generating your analysis!

USER: That's helpful, thanks! Generating now.
AGENT: Perfect! Let me know if you need anything else.

TAKEAWAYS:
✅ Acknowledged user's frustration
✅ Explained WHY the error occurred (not just HOW to fix it)
✅ Provided exact steps with where to click
✅ Confirmed the fix worked
✅ Offered proactive tip to prevent future issues
✅ Kept responses concise and scannable

Example 3: Handling Dissatisfaction with AI Analysis Quality
Context: User unhappy with generic AI analysis, feels it doesn't justify the cost.

USER: I just generated my first report and honestly, I'm disappointed. The AI analysis is super generic - it just says "CTR is good" and "CPA needs improvement". I could have written this myself in 5 minutes. Is this really what I'm paying for?
AGENT: I appreciate you sharing this honest feedback - it's really valuable. Let me address your concern and help you get better results.
You're right that the analysis should be more specific and insightful. Let me ask a few questions to understand what happened:

How many campaigns were in your CSV?
What was the date range of the data?
Did the campaigns have very similar performance (e.g., all CPAs within $2-3 of each other)?

The AI performs best with more data and clearer patterns. If it had limited data or all campaigns performed similarly, it can struggle to provide specific insights.

USER: It was only 3 campaigns over 7 days. They're all pretty new campaigns so the numbers are small - like $200 spend per campaign.
AGENT: Ah, that explains it! With only 3 campaigns over 7 days and low spend, there isn't much for the AI to analyze. It's like asking for a trend analysis when there's barely a data point.
Here's how to get much better insights:
Option 1: Export longer date range

Go back to Meta Ads Manager
Export last 30 days (or 90 days if campaigns have been running that long)
More data = clearer patterns = more specific insights

Option 2: Include more campaigns

If your client has 10+ campaigns, export all of them
AI can then compare performance and identify top/bottom performers
Single-campaign analysis is inherently limited

Option 3: Manually enhance the analysis
After AI generates the draft, click "Edit" and add your own insights:

Client-specific context the AI doesn't know
Strategic recommendations based on your expertise
Industry knowledge or competitive intel

The real value of InsightEngine isn't replacing your expertise - it's giving you a structured starting point so you're not staring at a blank page.
Would you be willing to try uploading 30 days of data with more campaigns? I think you'll see a dramatic quality improvement.

USER: Okay, that makes sense. I'll try exporting 30 days. But honestly, I thought the AI would be more advanced than this. Can it really not analyze 3 campaigns?
AGENT: That's a fair expectation! Let me be transparent about what the AI can and can't do:
What AI DOES well:

Identifying clear winners/losers when there's performance variance
Spotting unusual patterns (e.g., high CTR but low conversions)
Comparing campaigns against each other
Pulling out specific numbers and metrics
Structuring insights in a professional format

What AI STRUGGLES with:

Very small datasets (under 7-10 campaigns)
Short time periods (under 14 days)
Campaigns that all perform identically
Client-specific context it doesn't have access to

Why your report was generic:
With 3 similar-performing campaigns and 7 days of data, there simply wasn't much variance to analyze. It's like asking "which of these three identical apples is best?" - there's no meaningful answer.
Here's what I recommend:

Try the 30-day export with all campaigns (I think you'll be impressed)
Think of AI as your "first draft" - you add the expertise
If you're still not satisfied after testing with better data, let me know and we can discuss options (including refund if it's not a fit)

Fair enough?

USER: Alright, I'll give it another shot with more data. Uploading now... wow, this time it's WAY better. It's actually calling out specific campaigns by name and giving numbers. This is what I expected!
AGENT: Exactly! That's the difference quality data makes. Now you're seeing the AI at its best.
Going forward:

Always export at least 14-30 days of data
Include all active campaigns (even paused ones if they ran during the period)
The more variance in performance, the better the insights

And remember: you can ALWAYS edit the analysis before generating the PDF. Add your own commentary, adjust recommendations, or rewrite sections entirely. The AI is your starting point, not your final answer.
Does this report feel more like what you were expecting?

USER: Yes, much better. Thanks for explaining the data requirements - that wasn't clear to me initially. I think this will work for my needs.
AGENT: Glad we got there! And thanks for being open about your initial disappointment - it helps us understand where our onboarding needs to be clearer.
I'm going to make a note to:

Update our tutorial to emphasize the data requirements
Add a warning when uploads have < 5 campaigns or < 14 days

Your feedback directly improves the product for everyone!
Anything else I can help with today?

USER: Nope, all set. Appreciate the help!
AGENT: My pleasure! Enjoy your trial, and don't hesitate to reach out if questions come up.
Happy reporting! 📊

TAKEAWAYS:
✅ Didn't get defensive about criticism
✅ Asked diagnostic questions to understand root cause
✅ Explained AI limitations honestly (set realistic expectations)
✅ Provided actionable solution (better data input)
✅ Reframed value proposition (starting point vs replacement)
✅ Followed up to confirm satisfaction
✅ Turned negative feedback into product improvement opportunity
✅ Maintained empathetic tone throughout

Example 4: Proactive Escalation (Billing Issue)
Context: User's card declined, they're frustrated and threatening to cancel.

USER: This is ridiculous. I just got an email saying my payment failed, but I KNOW there's money on that card. I've been a customer for 3 months and now you're cutting off my access? I'm ready to cancel and find another tool.
AGENT: I'm really sorry for the frustration - I can see how this would be concerning, especially when you know the card is valid. Let me look into this immediately and get it resolved.
I'm pulling up your account now... okay, I see the declined payment from this morning. The decline reason from our payment processor (Stripe) is: "Do not honor - issuing bank declined"
This almost always means your bank's fraud prevention system flagged the charge, not that there's a problem with your account or funds. This is actually really common with recurring subscription charges.
Here's what's likely happening:
Your bank saw a recurring $99 charge from "Stripe / InsightEngine" and auto-declined it as a precaution. Banks do this when they detect patterns they consider risky (which recurring charges sometimes trigger).
Two ways to fix this:
Option A: Call your bank (fastest)

Call the number on the back of your card
Say: "I have a legitimate recurring charge from InsightEngine that was declined. Please allow it."
They'll remove the block immediately
Come back to Settings → Billing → "Retry Payment"
Should work within minutes

Option B: Use a different card

Go to Settings → Billing → "Update Payment Method"
Enter a different card (business card often works better than personal)
Subscription will auto-retry within the hour

Important: Your account isn't cut off yet - you have a 7-day grace period. Everything still works normally during that time.
Which option would you prefer to try?

USER: I don't want to call my bank. Can you just retry the payment on your end?
AGENT: I wish I could, but our payment processor (Stripe) won't let us manually retry a declined payment until either:

You update the payment method, OR
Your bank removes the block

However, I can do this:

Extend your grace period from 7 days to 14 days
This gives you two full weeks to resolve the payment issue without any interruption to service
You'll keep generating reports, no restrictions

I've just applied that extension to your account. You now have until {{date_14_days_out}} to update your payment.
Would you be willing to either call your bank or try a different card in the next day or two?

USER: Fine, I'll try a different card. Updating now... okay, it says "Payment method updated successfully". So I'm good?
AGENT: Yes! Let me confirm on my end... perfect, I see the new card ending in {{last_4}}. Your subscription is now active and you're all set.
What just happened:

Old card (ending in {{old_last_4}}) was declined by your bank
New card (ending in {{new_last_4}}) processed successfully
You won't be charged twice - the new charge replaces the failed one
Your billing date stays the same ({{billing_date}})

To prevent this in the future:
If you plan to keep using the old card for other things, you may want to call your bank and whitelist "Stripe / InsightEngine" so this doesn't happen next month.
Everything resolved to your satisfaction?

USER: Yeah, it's working now. Thanks for the extension, that helped reduce the stress.
AGENT: Of course! I completely understand the frustration when payments don't go through unexpectedly.
And just so you know - we never want to lose customers over payment processing issues. If this ever happens again, just reach out and we'll work with you to find a solution.
Thanks for sticking with us! Let me know if you need anything else.

USER: Will do. Appreciate the help.
AGENT: Anytime! Have a great day 📊

TAKEAWAYS:
✅ Acknowledged frustration immediately
✅ Investigated before responding (checked account details)
✅ Explained root cause clearly (bank's fraud prevention, not user error)
✅ Provided two clear solutions with pros/cons
✅ Offered proactive accommodation (grace period extension)
✅ Confirmed resolution and explained what happened
✅ Provided prevention tip for future
✅ De-escalated successfully - user went from threatening to cancel to satisfied

Example 5: Feature Request Handling
Context: User wants a feature that doesn't exist yet.

USER: Do you have Google Ads integration? I want to generate reports for my Google Ads campaigns too, not just Meta.
AGENT: Great question! We don't have Google Ads integration yet, but it's one of our most-requested features and is actively in development.
Current status:

Meta Ads: ✅ Available now
Google Ads: 🔄 Planned for Q2 2024 (April-June)
LinkedIn Ads: 🔄 Planned for Q3 2024
TikTok Ads: 🔄 Planned for Q4 2024

What this means for you:

You can use InsightEngine for Meta Ads reports today
Google Ads support coming in approximately 2-3 months
You'll automatically get access when we launch (no extra cost)

In the meantime, here's a workaround:
You CAN manually create a Google Ads CSV that matches our format:

Export from Google Ads (Campaigns → Download)
Rename columns to match our requirements:

Campaign → Campaign Name
Impr. → Impressions
Cost → Spend
Conv. → Conversions
Etc.


Upload to InsightEngine

It's not as seamless as native integration, but it works!
Want to be notified when Google Ads launches?
I can add you to our early access list - you'll get an email as soon as it's available, plus a chance to beta test before public launch.
Interested?

USER: Yes, add me to the early access list. But honestly, I need Google Ads support like... now. My clients are mostly running Google Ads, not Facebook.
AGENT: I completely understand - if most of your business is Google Ads, waiting 2-3 months isn't ideal.
Let me be honest about your options:
Option 1: Use the manual workaround I mentioned

Takes an extra 2-3 minutes per report (column renaming)
Not perfect, but gets you AI analysis for Google Ads data today
I can send you a detailed tutorial if you want to try this

Option 2: Wait for native Google Ads integration

Coming Q2 2024 (we're on track, not delayed)
Will be seamless (no manual column mapping)
You'll be in early access, so you'll get it before public launch

Option 3: Use InsightEngine for Meta + another tool for Google

Not ideal (paying for multiple tools)
But might be necessary short-term if workaround is too clunky

Option 4: Pause your subscription and come back when Google Ads launches

Cancel now (no penalty)
I'll email you personally when Google Ads is ready
You can reactivate and pick up where you left off

Which of these feels most realistic for your situation?

USER: I'll try the workaround for now, and if it's too annoying I'll just wait for the real integration. Can you send me that tutorial?
AGENT: Absolutely! I'm sending you a step-by-step guide via email right now.
What I'm sending:

Google Ads → InsightEngine CSV mapping guide (PDF)
Video walkthrough (3 minutes)
Sample template you can reuse
Script to automate the column renaming (if you're comfortable with spreadsheet formulas)

You should have it in your inbox within 2 minutes.
And I've added you to the Google Ads early access list - you'll hear from us the moment it's ready to test!
Anything else I can help with today?

USER: Nope, that's perfect. Thanks!
AGENT: My pleasure! Let me know how the workaround goes - if it's too painful, we can explore other options.
And seriously, thanks for the feedback. Every feature request helps us prioritize our roadmap!

TAKEAWAYS:
✅ Directly answered whether feature exists (no beating around bush)
✅ Provided clear timeline for when it's coming
✅ Offered workaround for immediate need
✅ Presented multiple options (not just "wait and see")
✅ Set realistic expectations about limitations
✅ Empowered user to make informed decision
✅ Followed through with promised resources
✅ Turned feature request into product feedback opportunity

9. GOLDEN DATASET (Evaluation Files)
Purpose of Golden Dataset
This dataset contains "perfect" question-answer pairs used to:

Test AI agent accuracy before deployment
Benchmark agent performance over time
Train new support staff on ideal responses
Identify knowledge gaps in documentation

Format
Each entry contains:

Question: Exactly as user would ask it
Expected Answer: The correct, complete response
Answer Type: Factual / Procedural / Troubleshooting / Opinion
Difficulty: Easy / Medium / Hard
Category: Product / Billing / Technical / Support
Success Criteria: How to evaluate if answer is correct


Golden Dataset: Product Questions

Entry 1
Question: What file formats do you support?
Expected Answer:
We only support CSV files. Excel files (.xlsx, .xls) need to be converted to CSV before uploading.
To convert Excel to CSV:

Open your Excel file
File → Save As
Choose "CSV (Comma delimited)"
Save and upload to InsightEngine

Answer Type: Factual
Difficulty: Easy
Category: Product
Success Criteria:
✅ States "CSV only"
✅ Mentions Excel is not supported
✅ Provides conversion instructions

Entry 2
Question: How many reports can I generate per month?
Expected Answer:
It depends on your plan:

Trial: 10 reports total (not per month)
Starter ($99/mo): 20 reports per month
Pro ($199/mo): 100 reports per month
Enterprise: Unlimited (custom pricing)

Your usage resets on the 1st of each month.
Answer Type: Factual
Difficulty: Easy
Category: Product
Success Criteria:
✅ Lists all plan limits accurately
✅ Mentions reset schedule
✅ Clarifies trial is total, not monthly

Entry 3
Question: Can I edit the AI-generated analysis?
Expected Answer:
Yes! After the AI generates the analysis, you can click "Edit" on any section (summary, wins, concerns, or recommendations) to modify the text.
Your edits will be reflected in the final PDF. This is useful for:

Adding client-specific context
Adjusting tone or recommendations
Correcting any AI misinterpretations

The AI provides a starting point - your expertise makes it great!
Answer Type: Factual + Procedural
Difficulty: Easy
Category: Product
Success Criteria:
✅ Confirms editing is possible
✅ Explains HOW to edit
✅ Explains WHY you'd want to edit
✅ Reinforces value proposition

Entry 4
Question: What advertising platforms do you support?
Expected Answer:
Currently, we only support Meta Ads (Facebook/Instagram) in v1.0.
Coming soon:

Google Ads: Q2 2024 (April-June)
LinkedIn Ads: Q3 2024
TikTok Ads: Q4 2024

If you need to generate reports for other platforms now, you can manually format your CSV to match our required columns (Campaign Name, Impressions, Clicks, Spend, Conversions, CPC, CTR, CPA). It requires some column renaming but works!
Want to be notified when new platforms launch? Email us at feedback@insightengine.com to join the early access list.
Answer Type: Factual
Difficulty: Medium
Category: Product
Success Criteria:
✅ Clearly states "Meta Ads only" for current version
✅ Provides roadmap dates for other platforms
✅ Offers workaround for immediate needs
✅ Provides path to stay updated

Entry 5
Question: Why does my report look different from my colleague's?
Expected Answer:
Each agency's reports reflect their unique branding:

Logo: Your uploaded agency logo appears in the header
Brand Color: Your chosen color is used for section headers and accents
Agency Name: Appears in the footer

If you want your reports to look identical to a colleague's:

Both of you go to Settings → Branding
Upload the same logo file
Use the same hex color code
Ensure agency names are spelled identically

The report structure (sections, layout, font) is the same for everyone - only branding differs.
Answer Type: Factual + Troubleshooting
Difficulty: Medium
Category: Product
Success Criteria:
✅ Explains branding causes differences
✅ Lists all customizable elements
✅ Provides solution to match reports
✅ Clarifies what stays consistent

Golden Dataset: Billing Questions

Entry 6
Question: How do I cancel my subscription?
Expected Answer:

Go to Settings → Billing
Click "Cancel Subscription"
Confirm cancellation

What happens after cancellation:

Your access continues until the end of your current billing period
After that, your account is paused (you can still view past reports, but can't generate new ones)
Your data is retained for 30 days in case you want to reactivate
After 30 days, all data is permanently deleted

No cancellation fees. No questions asked.
If you cancel by mistake, just reactivate within 30 days to restore full access.
Answer Type: Procedural
Difficulty: Easy
Category: Billing
Success Criteria:
✅ Provides exact steps to cancel
✅ Explains post-cancellation access clearly
✅ Mentions data retention period
✅ Reassures about no fees

Entry 7
Question: I was charged twice this month. What happened?
Expected Answer:
I understand the concern! Let me help you figure out what's happening.
Common reasons for "double charges":

Pending + Posted (same transaction):

Many banks show the same charge as both "pending" and "posted"
Wait 2-3 business days - the duplicate will disappear
Check your bank statement in a few days to confirm


Subscription + Overage:

One charge for your monthly subscription ($99 or $199)
Separate charge if you exceeded your report limit
Check Settings → Billing → Transaction History to see breakdown


Timing mismatch:

Your billing date is based on when you signed up, not the 1st of the month
If you signed up Jan 15, you're billed on the 15th of each month
Two charges in one calendar month can look like duplicates



If none of these apply:
Email support@insightengine.com with:

Screenshot of both charges from your bank
Transaction IDs from Settings → Billing
We'll investigate within 2 business days and issue a refund if it was an error

Answer Type: Troubleshooting
Difficulty: Medium
Category: Billing
Success Criteria:
✅ Acknowledges concern empathetically
✅ Explains most common (innocent) causes first
✅ Provides clear escalation path for genuine errors
✅ Specifies what info to include in support request

Entry 8
Question: Do you offer refunds?
Expected Answer:
Yes, in specific circumstances:
Eligible for full refund:

Service was completely unavailable for 48+ consecutive hours
You were charged in error (e.g., after canceling, or wrong amount)
Critical advertised feature not working for 7+ days despite reporting it

Eligible for partial refund (case-by-case):

Extended service degradation (72+ hours of significant issues)
Feature was advertised but never delivered

NOT eligible for refund:

"Changed my mind" after trial period ends
"Didn't use it enough" (trial is for testing)
"Found a cheaper alternative"
Reports generated successfully but you "didn't like the AI quality"

How to request a refund:
Email support@insightengine.com with:

Reason for refund request
Account email
Documentation of the issue (if technical problem)

We respond within 2 business days. Approved refunds process in 5-7 business days to your original payment method.
Answer Type: Factual / Policy
Difficulty: Medium
Category: Billing
Success Criteria:
✅ Clearly states circumstances for refunds
✅ Also clearly states what's NOT refundable
✅ Provides request process
✅ Sets response time expectations

Golden Dataset: Technical Questions

Entry 9
Question: Which browsers do you support?
Expected Answer:
Fully supported:

Chrome 90+
Firefox 88+
Safari 14+
Edge 90+

NOT supported:

Internet Explorer (any version)
Opera Mini
UC Browser

Mobile: Our website works on mobile browsers (iOS Safari, Chrome Android), but we recommend using desktop/laptop for the best experience, especially when uploading files.
Having issues?
Make sure your browser is updated to the latest version. Clear cache and cookies if you're experiencing slowness.
Answer Type: Factual
Difficulty: Easy
Category: Technical
Success Criteria:
✅ Lists supported browsers with version numbers
✅ Explicitly calls out unsupported browsers
✅ Mentions mobile experience
✅ Provides basic troubleshooting tip

Entry 10
Question: Can I use your API to integrate with my CRM?
Expected Answer:
Not yet, but API access is coming in Q3 2024!
What we're building:

RESTful API for generating reports programmatically
Webhook support for real-time notifications
Zapier integration (connect to 5,000+ apps)
Direct integrations with popular CRMs (HubSpot, Salesforce)

Who gets API access:

Pro Plan and above
Documentation will be available at docs.insightengine.com
Rate limits apply (fair usage)

Want early access?
Email api-preview@insightengine.com to join the beta program. We'll reach out when API testing begins (estimated May 2024).
Answer Type: Factual
Difficulty: Medium
Category: Technical
Success Criteria:
✅ Clearly states API doesn't exist yet
✅ Provides timeline
✅ Describes planned functionality
✅ Offers path to early access

Golden Dataset: Troubleshooting Questions

Entry 11
Question: My CSV upload is stuck at 0%. What do I do?
Expected Answer:
Let's troubleshoot this step-by-step:
First, quick checks:

File size: Is your CSV over 8MB? Large files can timeout. Try reducing date range or removing campaigns.
Internet connection: Test your speed at fast.com - you need at least 1 Mbps upload speed.
How long have you waited? Large files can take up to 2 minutes. If it's been under 2 minutes, keep waiting.

If still stuck after 5 minutes:
Solution 1: Cancel and retry

Close the upload modal
Wait 30 seconds
Try uploading again
Often works on second attempt

Solution 2: Try different browser

Upload issues are often browser-specific
Try Chrome (most reliable for file uploads)

Solution 3: Check file integrity

Open CSV in Excel/Google Sheets
Re-save as new CSV file
Try uploading the new file

Still not working?
Email the CSV to support@insightengine.com and we'll upload it manually for you. You'll have your report within 2 hours.
Answer Type: Troubleshooting
Difficulty: Medium
Category: Technical
Success Criteria:
✅ Provides diagnostic questions first
✅ Offers multiple solutions in priority order
✅ Each solution has clear steps
✅ Provides escalation path if all fails

Entry 12
Question: The AI said my CTR is bad, but it's actually 3%, which is above average. Why is it wrong?
Expected Answer:
Great catch - you're absolutely right that 3% CTR is above average for most industries!
This could be happening for a few reasons:
Possible Cause 1: Industry context
The AI uses general Meta Ads benchmarks (1.5-2.5% average). If your industry typically has higher CTRs (e.g., e-commerce often sees 3-5%), the AI wouldn't know that without context.
Possible Cause 2: Comparison within your data
If OTHER campaigns in your CSV have 4-5% CTR, the AI might flag 3% as "lower performing" relative to your own campaigns, not industry standards.
Possible Cause 3: AI misinterpretation
Occasionally the AI makes mistakes, especially with nuanced performance metrics.
How to fix this:
Solution 1: Edit the analysis
Click "Edit" on the analysis section and rewrite it to accurately reflect that 3% is strong:

"CTR of 3% exceeds industry average of 2%, indicating strong ad creative resonance"

Solution 2: Regenerate
Click "Try Again" to have AI re-analyze. Sometimes produces more accurate insights on second pass.
Solution 3: Report the issue
Click "Report Issue" button and describe the misinterpretation. This helps us improve AI prompts for future analyses.
Remember: Always review AI analysis before sending to clients. Your expertise trumps AI interpretation!
Answer Type: Troubleshooting + Educational
Difficulty: Hard
Category: Technical / Product
Success Criteria:
✅ Validates user's concern (acknowledges they're right)
✅ Explains WHY error occurred (multiple possibilities)
✅ Provides immediate solutions (edit or regenerate)
✅ Reinforces human-in-loop principle
✅ Offers feedback mechanism to improve product

Evaluation Criteria for Agent Responses
When testing an AI agent or human support agent against this golden dataset, evaluate responses using this rubric:
Scoring (per question):
3 points = Perfect

All success criteria met
Tone appropriate for context
No unnecessary information
Clear and actionable

2 points = Good

Most success criteria met
Minor omissions or unclear phrasing
Correct overall direction

1 point = Acceptable

Core answer correct but missing important details
Tone issues or excessive verbosity
User might need follow-up question

0 points = Fail

Factually incorrect
Missing key information
Confusing or unhelpful
Wrong tone (e.g., defensive when empathy needed)

Target Score: 90%+ (27/30 points across 10 random questions)

10. API DOCUMENTATION
InsightEngine API v1.0 Specification
Base URL: https://api.insightengine.com/v1
Authentication: Bearer token (JWT)
Format: JSON
Rate Limits: 100 requests/hour (Starter), 500 requests/hour (Pro)

Authentication
All API requests must include authentication header:
httpAuthorization: Bearer {your_api_token}
Get your API token:

Log into InsightEngine
Go to Settings → API
Click "Generate API Token"
Copy token (shown once only)

Token security:

Treat like a password - never commit to Git
Rotate tokens every 90 days
Revoke immediately if compromised


Endpoints

POST /reports/upload
Upload CSV file and create report draft.
Request:
httpPOST /v1/reports/upload
Content-Type: multipart/form-data

file: {csv_file}
client_id: "uuid-string"
Response (Success):
json{
  "status": "success",
  "report_id": "550e8400-e29b-41d4-a716-446655440000",
  "validation": {
    "rows_validated": 12,
    "total_spend": 2450.50,
    "total_conversions": 89,
    "date_range": "2024-01-01 to 2024-01-31"
  },
  "warnings": []
}
Response (Validation Error):
json{
  "status": "error",
  "error_code": "VALIDATION_FAILED",
  "error_message": "Missing required columns",
  "details": {
    "missing_columns": ["Conversions", "CPA"],
    "found_columns": ["Campaign Name", "Impressions", "Clicks"]
  },
  "fix_instructions": "Please re-export from Meta Ads with required columns"
}
Rate Limit: 20 uploads/hour
Max File Size: 10MB

POST /reports/{id}/analyze
Generate AI analysis for uploaded report.
Request:
httpPOST /v1/reports/550e8400-e29b-41d4-a716-446655440000/analyze
Content-Type: application/json

{}
Response (Success - Processing):
json{
  "status": "processing",
  "report_id": "550e8400-e29b-41d4-a716-446655440000",
  "estimated_completion": "2024-01-15T14:32:30Z",
  "message": "Analysis in progress. Poll this endpoint or wait for webhook."
}
Response (Success - Completed):
json{
  "status": "completed",
  "report_id": "550e8400-e29b-41d4-a716-446655440000",
  "analysis": {
    "summary": "Campaigns generated 89 conversions at an average CPA of $27.53...",
    "wins": [
      "Winter Sale campaign achieved CPA of $18.50, 33% below target",
      "CTR of 3.2% on Spring Launch indicates strong creative resonance",
      "Overall conversion volume increased 22% compared to previous period"
    ],
    "concerns": [
      "Summer Campaign spent $450 with zero conversions - immediate review needed",
      "Mobile CPA trending 15% higher than desktop across all campaigns   ],
    "recommendations": [
      "Increase daily budget on Winter Sale campaign by $200 to capture additional volume",
      "Pause Summer Campaign immediately and audit targeting parameters",
      "Launch mobile-specific creative test to address conversion rate gap"
    ]
  },
  "confidence_score": 0.92,
  "processing_time_ms": 18420
}
Response (Error - Idempotency Conflict):
json{
  "status": "error",
  "error_code": "ALREADY_PROCESSING",
  "error_message": "Report is already being analyzed",
  "report_id": "550e8400-e29b-41d4-a716-446655440000",
  "current_status": "processing",
  "started_at": "2024-01-15T14:30:00Z"
}
Rate Limit: 50 analyses/hour
Timeout: 30 seconds

POST /reports/{id}/generate-pdf
Generate branded PDF from completed analysis.
Request:
httpPOST /v1/reports/550e8400-e29b-41d4-a716-446655440000/generate-pdf
Content-Type: application/json

{}
Response (Success):
json{
  "status": "success",
  "report_id": "550e8400-e29b-41d4-a716-446655440000",
  "pdf_url": "https://storage.insightengine.com/reports/550e8400.pdf",
  "file_size_kb": 245,
  "expires_at": "2024-02-15T14:32:00Z",
  "download_url": "https://api.insightengine.com/v1/reports/550e8400/download"
}
Response (Error - Prerequisites Not Met):
json{
  "status": "error",
  "error_code": "ANALYSIS_NOT_COMPLETE",
  "error_message": "Cannot generate PDF - analysis must complete first",
  "report_id": "550e8400-e29b-41d4-a716-446655440000",
  "current_status": "processing"
}
Rate Limit: 100 PDF generations/hour
Timeout: 15 seconds

GET /reports/{id}
Retrieve report details and current status.
Request:
httpGET /v1/reports/550e8400-e29b-41d4-a716-446655440000
Response:
json{
  "report_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "completed",
  "client": {
    "id": "client-uuid",
    "name": "Acme Corp"
  },
  "created_at": "2024-01-15T14:25:00Z",
  "completed_at": "2024-01-15T14:32:00Z",
  "csv_filename": "meta_ads_export_jan2024.csv",
  "analysis": {
    "summary": "...",
    "wins": ["...", "...", "..."],
    "concerns": ["...", "..."],
    "recommendations": ["...", "...", "..."]
  },
  "pdf_url": "https://storage.insightengine.com/reports/550e8400.pdf",
  "metadata": {
    "campaigns_analyzed": 12,
    "total_spend": 2450.50,
    "date_range": "2024-01-01 to 2024-01-31"
  }
}
Rate Limit: 200 requests/hour

GET /reports
List all reports for authenticated agency.
Request:
httpGET /v1/reports?limit=20&offset=0&status=completed&client_id=uuid
Query Parameters:

limit (optional): Number of results (default: 20, max: 100)
offset (optional): Pagination offset (default: 0)
status (optional): Filter by status (draft, processing, completed, failed)
client_id (optional): Filter by client UUID

Response:
json{
  "reports": [
    {
      "report_id": "uuid-1",
      "client_name": "Acme Corp",
      "status": "completed",
      "created_at": "2024-01-15T14:25:00Z",
      "pdf_url": "https://storage.insightengine.com/reports/uuid-1.pdf"
    },
    {
      "report_id": "uuid-2",
      "client_name": "Beta LLC",
      "status": "completed",
      "created_at": "2024-01-12T10:15:00Z",
      "pdf_url": "https://storage.insightengine.com/reports/uuid-2.pdf"
    }
  ],
  "pagination": {
    "total": 45,
    "limit": 20,
    "offset": 0,
    "has_more": true
  }
}
Rate Limit: 100 requests/hour

DELETE /reports/{id}
Delete a report and associated files.
Request:
httpDELETE /v1/reports/550e8400-e29b-41d4-a716-446655440000
Response (Success):
json{
  "status": "success",
  "message": "Report deleted successfully",
  "report_id": "550e8400-e29b-41d4-a716-446655440000"
}
Response (Error - Not Found):
json{
  "status": "error",
  "error_code": "REPORT_NOT_FOUND",
  "error_message": "Report does not exist or you don't have permission to delete it"
}
Rate Limit: 50 deletions/hour

GET /clients
List all clients for authenticated agency.
Request:
httpGET /v1/clients?limit=50&offset=0
Response:
json{
  "clients": [
    {
      "id": "client-uuid-1",
      "name": "Acme Corp",
      "website": "acmecorp.com",
      "industry": "E-commerce",
      "created_at": "2024-01-01T10:00:00Z",
      "report_count": 12
    },
    {
      "id": "client-uuid-2",
      "name": "Beta LLC",
      "website": "betallc.com",
      "industry": "SaaS",
      "created_at": "2023-12-15T08:30:00Z",
      "report_count": 8
    }
  ],
  "pagination": {
    "total": 25,
    "limit": 50,
    "offset": 0,
    "has_more": false
  }
}
Rate Limit: 100 requests/hour

POST /clients
Create a new client.
Request:
httpPOST /v1/clients
Content-Type: application/json

{
  "name": "Gamma Inc",
  "website": "gammainc.com",
  "industry": "Healthcare"
}
Response (Success):
json{
  "status": "success",
  "client": {
    "id": "client-uuid-3",
    "name": "Gamma Inc",
    "website": "gammainc.com",
    "industry": "Healthcare",
    "created_at": "2024-01-15T14:45:00Z"
  }
}
Response (Error - Duplicate):
json{
  "status": "error",
  "error_code": "CLIENT_ALREADY_EXISTS",
  "error_message": "A client with this name already exists",
  "existing_client_id": "client-uuid-existing"
}
Rate Limit: 50 creations/hour

GET /usage
Get current usage statistics and limits.
Request:
httpGET /v1/usage
Response:
json{
  "current_period": {
    "start_date": "2024-01-01",
    "end_date": "2024-01-31",
    "reports_used": 15,
    "reports_limit": 20,
    "reports_remaining": 5,
    "usage_percentage": 75
  },
  "plan": {
    "name": "Starter",
    "price": 99,
    "billing_cycle": "monthly",
    "next_billing_date": "2024-02-01"
  },
  "rate_limits": {
    "api_requests_per_hour": 100,
    "current_hour_usage": 23
  }
}
Rate Limit: 200 requests/hour

Webhooks
Subscribe to real-time events via webhooks.
Supported Events:

report.analysis.completed - Analysis finished successfully
report.analysis.failed - Analysis failed after retries
report.pdf.generated - PDF generated successfully
report.pdf.failed - PDF generation failed
usage.limit.warning - Approaching monthly limit (90%)
usage.limit.reached - Monthly limit reached

Webhook Payload Example:
json{
  "event": "report.analysis.completed",
  "timestamp": "2024-01-15T14:32:00Z",
  "data": {
    "report_id": "550e8400-e29b-41d4-a716-446655440000",
    "client_name": "Acme Corp",
    "status": "completed",
    "analysis_url": "https://api.insightengine.com/v1/reports/550e8400"
  },
  "signature": "sha256_hmac_signature_here"
}
Webhook Configuration:

Go to Settings → API → Webhooks
Add your webhook URL
Select events to subscribe to
Copy webhook secret for signature verification
Save configuration

Verify Webhook Signature (Node.js):
javascriptconst crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const expectedSignature = crypto
    .createHmac('sha256', secret)
    .update(JSON.stringify(payload))
    .digest('hex');
  
  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expectedSignature)
  );
}

Error Codes Reference
CodeHTTP StatusMeaningVALIDATION_FAILED400CSV validation error (missing columns, invalid data)ALREADY_PROCESSING409Idempotency conflict - operation already in progressANALYSIS_NOT_COMPLETE400Tried to generate PDF before analysis completedREPORT_NOT_FOUND404Report doesn't exist or no permissionCLIENT_ALREADY_EXISTS409Client with same name already existsUSAGE_LIMIT_EXCEEDED429Monthly report limit reachedRATE_LIMIT_EXCEEDED429API rate limit exceededINVALID_TOKEN401API token invalid or expiredINSUFFICIENT_PERMISSIONS403Token doesn't have required permissionsFILE_TOO_LARGE413CSV exceeds 10MB limitINTERNAL_ERROR500Server error (contact support if persists)

Rate Limiting
Headers in every response:
httpX-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1705330800
When limit exceeded:
httpHTTP/1.1 429 Too Many Requests
Retry-After: 3600

{
  "error_code": "RATE_LIMIT_EXCEEDED",
  "error_message": "API rate limit exceeded. Resets in 1 hour.",
  "limit": 100,
  "reset_at": "2024-01-15T15:00:00Z"
}
Rate Limits by Plan:

Starter: 100 requests/hour
Pro: 500 requests/hour
Enterprise: 2000 requests/hour (custom)

Best Practices:

Implement exponential backoff on 429 responses
Cache GET responses when possible
Use webhooks instead of polling for status
Batch operations when feasible


Pagination
All list endpoints support pagination via limit and offset:
httpGET /v1/reports?limit=20&offset=40
Response includes pagination metadata:
json{
  "pagination": {
    "total": 125,
    "limit": 20,
    "offset": 40,
    "has_more": true,
    "next_offset": 60
  }
}

SDK Libraries
Official SDKs:

JavaScript/Node.js: npm install @insightengine/sdk
Python: pip install insightengine
Ruby: gem install insightengine

Example (Node.js):
javascriptconst InsightEngine = require('@insightengine/sdk');

const client = new InsightEngine({
  apiToken: process.env.INSIGHTENGINE_API_TOKEN
});

// Upload CSV and generate report
async function generateReport(csvPath, clientId) {
  const report = await client.reports.upload({
    file: csvPath,
    clientId: clientId
  });
  
  console.log('Uploaded:', report.id);
  
  const analysis = await client.reports.analyze(report.id);
  console.log('Analysis:', analysis.summary);
  
  const pdf = await client.reports.generatePDF(report.id);
  console.log('PDF URL:', pdf.url);
}

11. DATA FIELD SPECIFICATIONS
CSV Input Schema
Required Columns (Exact Names, Case-Sensitive)
Column NameData TypeConstraintsExampleDescriptionCampaign NameStringNon-empty, max 255 chars"Winter Sale 2024"Name of the ad campaignImpressionsInteger> 045000Number of times ads were displayedClicksInteger>= 0, <= Impressions1200Number of clicks on adsSpendDecimal>= 0, max 2 decimals850.00Total amount spent in USDConversionsInteger>= 0, <= Clicks45Number of conversion eventsCPCDecimal> 0, max 2 decimals0.71Cost per click in USDCTRDecimal0 < CTR < 100, max 2 decimals2.67Click-through rate as percentageCPADecimal> 0 OR null, max 2 decimals18.89Cost per acquisition in USD (null if 0 conversions)
Optional Columns (Ignored but Allowed)
These columns can be present in CSV but are not used in analysis:

Date columns (Start Date, End Date, etc.)
Demographics (Age, Gender, etc.)
Placements (Feed, Stories, etc.)
Device types (Mobile, Desktop)
Geographic data (Country, Region)
Any other Meta Ads metrics


Database Schema - Reports Table
reports
FieldTypeNullableDefaultDescriptionidUUIDNouuid_generate_v4()Primary keyagency_idUUIDNo-Foreign key to agencies tableclient_idUUIDNo-Foreign key to clients tablecsv_filenameTextNo-Original filename of uploaded CSVparsed_dataJSONBNo-Validated CSV data as JSON arrayai_analysisJSONBYesnullAI-generated analysis (structured)pdf_urlTextYesnullPublic URL to generated PDFstatusTextNo'draft'Enum: draft, processing, completed, failederror_messageTextYesnullError details if status=failedprocessing_started_atTimestamptzYesnullWhen analysis begancreated_atTimestamptzNonow()When report was createdcompleted_atTimestamptzYesnullWhen report finished processingretry_countIntegerNo0Number of retry attempts
Indexes
sqlCREATE INDEX idx_reports_agency_id ON reports(agency_id);
CREATE INDEX idx_reports_client_id ON reports(client_id);
CREATE INDEX idx_reports_status ON reports(status);
CREATE INDEX idx_reports_created_at ON reports(created_at DESC);
CREATE UNIQUE INDEX idx_processing_lock ON reports(id) WHERE status = 'processing';

Database Schema - Agencies Table
agencies
FieldTypeNullableDefaultDescriptionidUUIDNouuid_generate_v4()Primary keyuser_idUUIDNo-Foreign key to auth.users (Supabase Auth)agency_nameTextNo-Display name of agencylogo_urlTextYesnullPublic URL to agency logobrand_colorTextNo'#6366f1'Hex color code for brandingstripe_customer_idTextYesnullStripe customer identifierstripe_subscription_idTextYesnullStripe subscription identifiersubscription_statusTextNo'trialing'Enum: trialing, active, canceled, past_dueplan_typeTextNo'starter'Enum: starter, pro, enterprisereports_used_this_monthIntegerNo0Current month's usage counterreport_limitIntegerNo20Monthly report limit based on plancreated_atTimestamptzNonow()When agency account was createdupdated_atTimestamptzNonow()Last update timestamp
Indexes
sqlCREATE UNIQUE INDEX idx_agencies_user_id ON agencies(user_id);
CREATE UNIQUE INDEX idx_agencies_stripe_customer ON agencies(stripe_customer_id);

Database Schema - Clients Table
clients
FieldTypeNullableDefaultDescriptionidUUIDNouuid_generate_v4()Primary keyagency_idUUIDNo-Foreign key to agencies tableclient_nameTextNo-Client business namewebsite_urlTextYesnullClient website URLindustryTextYesnullClient industry (free text)created_atTimestamptzNonow()When client was added
Indexes
sqlCREATE INDEX idx_clients_agency_id ON clients(agency_id);
CREATE INDEX idx_clients_name ON clients(client_name);

AI Analysis JSON Schema
Structure (ai_analysis JSONB field)
json{
  "summary": "String (2-3 sentences, 150-300 characters)",
  "wins": [
    "String (specific campaign win with metric)",
    "String (specific campaign win with metric)",
    "String (specific campaign win with metric)"
  ],
  "concerns": [
    "String (specific issue with campaign + metric)",
    "String (specific issue with campaign + metric)"
  ],
  "recommendations": [
    "String (actionable step with campaign name)",
    "String (actionable step with campaign name)",
    "String (actionable step with campaign name)"
  ]
}
Validation Rules
javascriptconst schema = {
  type: "object",
  required: ["summary", "wins", "concerns", "recommendations"],
  properties: {
    summary: {
      type: "string",
      minLength: 150,
      maxLength: 500
    },
    wins: {
      type: "array",
      minItems: 3,
      maxItems: 3,
      items: {
        type: "string",
        minLength: 30,
        maxLength: 200
      }
    },
    concerns: {
      type: "array",
      minItems: 2,
      maxItems: 2,
      items: {
        type: "string",
        minLength: 30,
        maxLength: 200
      }
    },
    recommendations: {
      type: "array",
      minItems: 3,
      maxItems: 3,
      items: {
        type: "string",
        minLength: 30,
        maxLength: 250
      }
    }
  }
};

Parsed CSV Data JSON Schema
Structure (parsed_data JSONB field)
json[
  {
    "Campaign Name": "Winter Sale 2024",
    "Impressions": 45000,
    "Clicks": 1200,
    "Spend": 850.00,
    "Conversions": 45,
    "CPC": 0.71,
    "CTR": 2.67,
    "CPA": 18.89
  },
  {
    "Campaign Name": "Spring Launch",
    "Impressions": 32000,
    "Clicks": 890,
    "Spend": 620.00,
    "Conversions": 32,
    "CPC": 0.70,
    "CTR": 2.78,
    "CPA": 19.38
  }
]
Notes:

Array of objects, one per campaign
Keys match CSV column names exactly
Numeric values are stored as numbers (not strings)
CPA can be null if Conversions = 0


12. TOOL AUTHORIZATION MATRIX
Purpose
Define which AI agents and system components have permission to use specific tools, APIs, and operations.

Agent Access Levels
Agent/ComponentAccess LevelDescriptionCSV Validator AgentRead-OnlyCan read uploaded files, validate structure, no database writesAI Analysis AgentAPI + WriteCan call OpenAI API, write to reports.ai_analysis fieldPDF Generator AgentStorage + WriteCan write to Supabase Storage, update reports.pdf_urlUser InterfaceFull CRUDAll operations on own agency's data via Row Level SecurityWebhook ProcessorEvent DispatchCan trigger webhooks, no data modificationBackground JobsLimited WriteCan update status fields, increment counters

Tool Authorization Matrix
External APIs
Tool/APIWho Can UseOperations AllowedRate LimitCost TrackingOpenAI API (GPT-4o-mini)AI Analysis Agent onlycreate_completion50 req/minYes - track tokensStripe APIBilling Service onlycreate_customer, create_subscription, retrieve_invoice100 req/secN/APostmark APIEmail Service onlysend_email10,000/dayYes - track emails
Authorization Method: Environment variables (API keys), never exposed to client

Database Operations
TableAgentSELECTINSERTUPDATEDELETENotesreportsCSV Validator✅✅❌❌Can only create draftsreportsAI Analysis Agent✅❌✅❌Can only update ai_analysis + statusreportsPDF Generator✅❌✅❌Can only update pdf_urlreportsUser (via RLS)✅✅✅✅Own agency data onlyagenciesBilling Service✅❌✅❌Can update usage + subscriptionagenciesUser (via RLS)✅❌✅❌Own agency only, limited fieldsclientsUser (via RLS)✅✅✅✅Own agency's clients only
Authorization Method: Supabase Row Level Security (RLS) + Service role keys for agents

Storage Operations
BucketWho Can UseUploadReadDeletePublic Accessreports (PDFs)PDF Generator✅❌❌Yes (public URLs)reportsUsers (via RLS)❌✅✅Yes (own reports)logosUsers (via RLS)✅✅✅Yes (own logo)logosPDF Generator❌✅❌Yes (for rendering)
Authorization Method: Supabase Storage RLS policies

Critical Operations (Require Human Approval)
OperationRequires Approval FromReasonDelete user account + dataUser themselves OR Support ManagerGDPR complianceIssue refund > $50Support ManagerFinancial controlsOverride rate limitsEngineering LeadSystem integrityAccess production database directlyCTO + documented reasonData securityModify RLS policiesEngineering LeadSecurity criticalDeploy schema changesCTO approval + peer reviewDatabase integrity

Service Account Permissions
Service Account: ai-analysis-service
Purpose: Execute AI analysis workflow
Permissions:
yamldatabase:
  - reports: SELECT, UPDATE (ai_analysis, status, completed_at fields only)
  - agencies: SELECT (for usage limit checks)

apis:
  - openai: FULL_ACCESS
  
environment_variables:
  - OPENAI_API_KEY: READ
  - DATABASE_URL: READ
  
rate_limits:
  - 50 OpenAI requests/minute
  - 100 database queries/minute

Service Account: pdf-generator-service
Purpose: Generate and store PDF files
Permissions:
yamldatabase:
  - reports: SELECT, UPDATE (pdf_url field only)
  - agencies: SELECT (for logo_url, brand_color)
  - clients: SELECT (for client_name)

storage:
  - reports bucket: UPLOAD, GET_PUBLIC_URL
  - logos bucket: READ_ONLY

rate_limits:
  - 100 PDF generations/minute
  - 500 storage operations/minute

Service Account: webhook-dispatcher
Purpose: Send webhook notifications to users
Permissions:
yamldatabase:
  - reports: SELECT (read-only)
  - agencies: SELECT (for webhook URLs)

apis:
  - postmark: SEND_EMAIL
  - user_webhooks: POST (external URLs)

rate_limits:
  - 10,000 webhooks/day per agency
  - 1,000 emails/day per agency

Security Boundaries
What Agents CANNOT Do (Hard Restrictions)
❌ CSV Validator Agent CANNOT:

Call OpenAI API
Access other agencies' data
Modify existing reports
Access Stripe API

❌ AI Analysis Agent CANNOT:

Generate PDFs
Delete reports
Access Stripe API
Modify agency settings

❌ PDF Generator Agent CANNOT:

Trigger AI analysis
Delete reports
Modify agency branding settings
Access payment information

❌ No Agent Can EVER:

Bypass Row Level Security
Access auth.users table directly
Modify audit logs
Disable rate limits
Execute arbitrary SQL


Audit Logging
All privileged operations are logged to system_audit_log table:
sqlCREATE TABLE system_audit_log (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  timestamp TIMESTAMPTZ DEFAULT NOW(),
  service_account TEXT NOT NULL,
  operation TEXT NOT NULL,
  resource_type TEXT NOT NULL,
  resource_id TEXT,
  ip_address INET,
  user_agent TEXT,
  success BOOLEAN,
  error_message TEXT,
  metadata JSONB
);
Logged Operations:

All database writes by service accounts
All external API calls (OpenAI, Stripe)
All file uploads/deletes
All failed authentication attempts
All rate limit violations


Emergency Access Procedures
Production Database Access (Break Glass)
When Needed:

Critical data corruption
Emergency user data deletion (GDPR)
Investigation of security incident

Process:

Engineer creates ticket with justification
CTO approves in writing (Slack message insufficient)
Access granted for 1 hour maximum
All queries logged and reviewed
Post-mortem required within 24 hours

How:
bash# Request temporary credentials (requires 2FA)
$ vault login -method=okta
$ vault read database/creds/emergency-access
# Credentials expire in 1 hour

This completes the InsightEngine operational documentation package. All 12 sections are now production-ready for AI agent implementation and team onboarding.
