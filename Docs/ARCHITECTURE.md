# System Architecture Overview

## 🏗️ High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         EMAIL SOURCES                                │
├─────────────────────────────────────────────────────────────────────┤
│  • Gmail Inbox                                                       │
│  • Website Contact Forms (dadsprinting.com, proteamjerseys.com)     │
│  • Customer Replies                                                  │
│  • Quote Requests                                                    │
└───────────────────────────┬─────────────────────────────────────────┘
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    EMAIL TRIAGE SYSTEM (Core)                        │
├─────────────────────────────────────────────────────────────────────┤
│  • Content Analysis                                                  │
│  • Sender Classification                                             │
│  • Intent Detection                                                  │
│  • Priority Scoring                                                  │
│  • Rules Engine (email_rules_master_clean.csv)                      │
└───────────────────────────┬─────────────────────────────────────────┘
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    LABEL LOGIC CONTROLLER                            │
├─────────────────────────────────────────────────────────────────────┤
│  • Gmail Label Management                                            │
│  • State Transitions                                                 │
│  • Workflow Triggers                                                 │
└───────────────────────────┬─────────────────────────────────────────┘
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    ROUTING & PROCESSING                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ┌─────────────────────┐  ┌─────────────────────┐                  │
│  │ AUTOMATED RESPONSES │  │  HUMAN ROUTING      │                  │
│  ├─────────────────────┤  ├─────────────────────┤                  │
│  │ • FAQ Bot           │  │ • Sales Team        │                  │
│  │ • Acknowledgments   │  │ • Support Team      │                  │
│  │ • Status Updates    │  │ • Quote Specialists │                  │
│  │ • Follow-ups        │  │ • Management        │                  │
│  └─────────────────────┘  └─────────────────────┘                  │
│                                                                       │
└───────────────────────────┬─────────────────────────────────────────┘
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    EXTERNAL INTEGRATIONS                             │
├─────────────────────────────────────────────────────────────────────┤
│  • Pipedrive CRM (Contact Management)                               │
│  • Gmail (Label & Thread Management)                                │
│  • Website Forms (Data Capture)                                     │
│  • Invoice Systems (Payment Tracking)                               │
└───────────────────────────┬─────────────────────────────────────────┘
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────────┐
│                 MONITORING & MAINTENANCE                             │
├─────────────────────────────────────────────────────────────────────┤
│  • Catch-All Recovery (Hourly)                                      │
│  • Label Cleanup (Daily)                                             │
│  • Form Email Cleaner (Hourly)                                      │
│  • Performance Audits (AI TRIAGE AUDIT)                             │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Email Lifecycle Flow

```
1. EMAIL ARRIVES
   │
   ├─→ [Website Form] ──→ Form Router ──→ Pipedrive + Autoresponder
   │
   └─→ [Direct Email]
        │
        ↓
2. INITIAL TRIAGE
   │
   ├─→ Spam Check ──→ [REJECT/FILTER]
   │
   └─→ Classification
        │
        ├─→ Quote Request
        ├─→ Customer Service
        ├─→ Follow-up Required
        ├─→ FAQ/Simple Query
        └─→ Other
        │
        ↓
3. LABEL APPLICATION
   │
   ├─→ Category Label (Quote/Support/Info)
   ├─→ Status Label (New/Pending/Resolved)
   ├─→ Priority Label (High/Medium/Low)
   └─→ Team Assignment Label
        │
        ↓
4. ROUTING DECISION
   │
   ├─→ [Automated] ──→ FAQ Bot ──→ Send Response ──→ Label: Resolved
   │
   ├─→ [Semi-Auto] ──→ Draft Response ──→ Human Review ──→ Send
   │
   └─→ [Human] ──→ Assign Team Member ──→ Label: Pending
        │
        ↓
5. TRACKING & FOLLOW-UP
   │
   ├─→ Monitor Response Time
   ├─→ Track Conversation State
   ├─→ Auto-Follow-up if No Response
   └─→ Update CRM Records
        │
        ↓
6. RESOLUTION
   │
   ├─→ Mark Resolved
   ├─→ Archive Thread
   ├─→ Update Audit Log
   └─→ Customer Satisfaction Check
```

---

## 🤖 Workflow Dependencies

```
EMAIL TRIAGE SYSTEM (Master)
│
├─→ DEPENDS ON: Label Logic Controller
│   └─→ TRIGGERS: All downstream workflows
│
├─→ TRIGGERS ON EMAIL TYPE:
│   ├─→ Quote Request ──→ Comms Agent - Needs Info to Quote
│   ├─→ Customer Inquiry ──→ Comms Agent - Customer Service Router
│   ├─→ Simple FAQ ──→ Comms Agent - FAQ Bot
│   └─→ Quote Rejection ──→ Comms Agent - Quote Rejected
│
├─→ FORM SUBMISSIONS:
│   ├─→ Contact Form ──→ Pipedrive Integration
│   └─→ Contact Form ──→ Autoresponder & Router
│
├─→ MAINTENANCE:
│   ├─→ HOURLY: Form Email Cleaner
│   ├─→ HOURLY: Missed Email Recovery (Catch-All)
│   └─→ DAILY: Label Cleanup
│
└─→ PAYMENT TRACKING:
    └─→ Invoice Paid ──→ Auto Forwarder
```

---

## 📊 State Machine (Email States)

```
┌─────────────┐
│   RECEIVED  │ (New email arrives)
└──────┬──────┘
       │
       ↓
┌─────────────┐
│  CLASSIFIED │ (Triage assigns category)
└──────┬──────┘
       │
       ├─→ [FAQ] ──→ AUTO_RESPONDED ──→ RESOLVED
       │
       ├─→ [Simple] ──→ PENDING_AUTO ──→ SENT ──→ RESOLVED
       │
       └─→ [Complex]
            │
            ↓
       ┌─────────────┐
       │  ASSIGNED   │ (Routed to team member)
       └──────┬──────┘
              │
              ↓
       ┌─────────────┐
       │   PENDING   │ (Awaiting response)
       └──────┬──────┘
              │
              ├─→ [No Info Needed] ──→ REPLIED ──→ RESOLVED
              │
              └─→ [Info Needed] ──→ WAITING_CUSTOMER
                   │
                   ↓
              ┌─────────────┐
              │   REPLIED   │ (Customer responds)
              └──────┬──────┘
                     │
                     ├─→ [More Info] ──→ PENDING (loop)
                     │
                     └─→ [Complete] ──→ RESOLVED
                          │
                          ↓
                     ┌─────────────┐
                     │  ARCHIVED   │ (Final state)
                     └─────────────┘
```

---

## 🔧 Technical Stack

### Automation Platform
- **Make.com** (formerly Integromat)
  - Webhook-based triggers
  - JSON scenario definitions
  - API integrations

### Email Infrastructure
- **Gmail API**
  - Label management
  - Thread reading/writing
  - Search and filtering

### CRM
- **Pipedrive**
  - Contact creation
  - Deal tracking
  - Activity logging

### Data Storage
- **CSV Files** - Classification rules
- **Excel Spreadsheets** - Tag-action mappings
- **JSON Files** - Workflow definitions
- **RTF Documents** - System documentation

---

## 📈 Scalability Considerations

### Current Capacity
- Handles multiple emails per minute
- Processes form submissions in real-time
- Maintains state across 100+ concurrent conversations

### Growth Path
1. **Phase 1** (Current): Single-business automation
2. **Phase 2**: Multi-business support (dadsprinting.com + proteamjerseys.com)
3. **Phase 3**: Additional business integration
4. **Phase 4**: White-label solution for other businesses

### Bottlenecks to Monitor
- Make.com operation limits
- Gmail API quotas
- Pipedrive API rate limits
- Manual review queue size

---

## 🛡️ Error Handling

### Failure Recovery
```
Error Occurs
│
├─→ [Workflow Failure] ──→ Catch-All Recovery ──→ Manual Review
│
├─→ [Classification Error] ──→ Default Routing ──→ Human Triage
│
├─→ [API Timeout] ──→ Retry Logic (3x) ──→ Error Log
│
└─→ [Data Error] ──→ Validation Failed ──→ Alert + Manual Fix
```

### Monitoring Points
1. Emails in "Unclassified" label
2. Workflows with failed runs
3. Emails older than 24h without response
4. API error rates
5. Customer reply times

---

**Document Version**: 1.0  
**Last Updated**: February 2026
