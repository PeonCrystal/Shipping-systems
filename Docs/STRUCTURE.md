# Repository Structure Guide

This document provides a detailed breakdown of the emailtriagesystem repository structure and the purpose of each component.

## 📁 Directory Structure

### `/docs` - System Documentation
Contains comprehensive technical documentation about the email triage system.

| File | Purpose |
|------|---------|
| `readme.md` | Documentation directory overview |
| `change_log.md.rtf` | Historical record of system changes and updates |
| `failure_modes.rtf` | Documentation of known failure scenarios and recovery procedures |
| `label_state_machine.rtf` | Email labeling logic and state transitions |
| `system_architecture.d.rtf` | System architecture diagrams and data flow documentation |

**Use Case**: Reference these documents when troubleshooting, understanding system behavior, or planning modifications.

---

### `/rules` - Email Classification Rules
Contains the business logic for email categorization and routing.

| File | Purpose |
|------|---------|
| `readme.md` | Rules directory overview |
| `email_rules_master_clean.csv` | Master list of email classification rules |
| `Email Tags W_Associated Actions (1).xlsx` | Detailed tag-to-action mapping spreadsheet |

**Use Case**: Modify these files to add new email categories, change routing logic, or update automated responses.

---

### `/workflows` - Make.com Automation Scenarios
JSON files that define Make.com (Integromat) automation workflows. Each file represents a complete automation scenario.

#### Core Triage System
| File | Purpose |
|------|---------|
| `EMAIL TRIAGE SYSTEM - - TIGHTEN.json` | Main email classification and routing engine |
| `Label Logic Controller (1).json` | Manages Gmail label application and state transitions |

#### Customer Service Agents
| File | Purpose |
|------|---------|
| `Comms Agent - Customer Service Inquiry -_Correct Team Member (1).json` | Routes customer inquiries to appropriate team members |
| `Comms Agent - Needs Info to Quote.json` | Handles quote requests requiring additional information |
| `Comms Agent - Quote Rejected.json` | Manages rejected quote follow-ups |
| `Comms Agent - State 3 - FAQ Bot (1).json` | Automated FAQ response system |

#### Form Processing
| File | Purpose |
|------|---------|
| `Contact Us Form (Website) to Pipedrive Uncontacted (2).json` | Website contact form to CRM integration |
| `Contact Us Form - Autoresponder & Router - COMMS (2).json` | Automated response to web form submissions |

#### Communication Management
| File | Purpose |
|------|---------|
| `COMMS - Following Up Reply Automator.json` | Automated follow-up on pending conversations |
| `Invoice Paid Auto Forwarder (1).json` | Forwards paid invoice notifications |

#### Maintenance & Recovery
| File | Purpose |
|------|---------|
| `CATCH-ALL - Missed Email Recovery.json` | Recovers emails that fell through the cracks |
| `Hourly Form Email Cleaner.json` | Regular cleanup of spam/invalid form emails |
| `TAG-SYS Label Cleanup (Daily) (1).json` | Daily label maintenance and optimization |

**Use Case**: Import these JSON files into Make.com to replicate or modify automation scenarios.

---

## 📄 Root Level Files

### Documentation Files

| File | Purpose |
|------|---------|
| `README.md` | Main repository overview and quick start guide |
| `STRUCTURE.md` | This file - detailed structural documentation |
| `SETUP.md` | Step-by-step system setup instructions |
| `RULES.md` | Business rules and decision logic documentation |
| `CONTRIBUTING.md` | Guidelines for modifying and extending the system |
| `CHANGELOG.md` | Version history and recent updates |

### AI Context Files

| File | Purpose |
|------|---------|
| `CLAUDE_CONTEXT.md` | Primary AI assistant context and system understanding |
| `Claude_context.md` | Additional AI context and prompts |
| `clinerules.md` | Command-line interface rules and automation guidelines |

**Use Case**: These files help AI assistants understand the system quickly for troubleshooting and modifications.

### System Files

| File | Purpose |
|------|---------|
| `gitignore` | Git ignore patterns for sensitive data |
| `Rules` | Quick reference for business rules |
| `read_me.rtf` | Legacy readme file |

### Audit & Analysis

| File | Purpose |
|------|---------|
| `AI TRIAGE AUDIT - Sheet1.csv` | System performance metrics and audit logs |

**Use Case**: Analyze system performance, identify bottlenecks, and track improvements over time.

---

## 🔄 Data Flow

```
┌─────────────────┐
│  Incoming Email │
└────────┬────────┘
         │
         ↓
┌─────────────────────────────┐
│ EMAIL TRIAGE SYSTEM         │
│ (Classification Engine)      │
└────────┬────────────────────┘
         │
         ↓
┌─────────────────────────────┐
│ Label Logic Controller       │
│ (State Management)           │
└────────┬────────────────────┘
         │
         ├──→ Customer Service Agents
         ├──→ Form Processors
         ├──→ Automated Responses
         ├──→ CRM Integration
         │
         ↓
┌─────────────────────────────┐
│ Monitoring & Recovery        │
│ (Catch-All, Cleanup)         │
└─────────────────────────────┘
```

---

## 🗂️ File Naming Conventions

### Workflows
- **Pattern**: `[CATEGORY] - [Description] [(Version)].json`
- **Examples**: 
  - `COMMS - Following Up Reply Automator.json`
  - `Comms Agent - Quote Rejected.json`
  - `EMAIL TRIAGE SYSTEM - - TIGHTEN.json`

### Documentation
- **Pattern**: `[name].[extension]` or `[name].md.[extension]`
- **Examples**: 
  - `change_log.md.rtf`
  - `system_architecture.d.rtf`

### Rules & Data
- **Pattern**: `[description].[extension]`
- **Examples**: 
  - `email_rules_master_clean.csv`
  - `AI TRIAGE AUDIT - Sheet1.csv`

---

## 📊 Integration Points

### External Services
1. **Gmail** - Email interface and label management
2. **Make.com** - Workflow automation platform
3. **Pipedrive** - CRM for contact management
4. **Website Forms** - Customer inquiry capture

### Data Sources
1. **Email Classification Rules** (`/rules`)
2. **Workflow Definitions** (`/workflows`)
3. **System Documentation** (`/docs`)
4. **Audit Logs** (Root CSV files)

---

## 🔐 Sensitive Information

The following items should **NEVER** be committed to the repository:
- API keys and tokens
- Email credentials
- Customer data
- Personal information
- Make.com webhook URLs (in production)

Refer to `.gitignore` for the complete exclusion list.

---

## 🛠️ Maintenance Guidelines

### Adding New Workflows
1. Create workflow in Make.com
2. Test thoroughly in sandbox environment
3. Export as JSON
4. Add to `/workflows` directory with descriptive name
5. Update this structure document
6. Document in `CHANGELOG.md`

### Modifying Classification Rules
1. Update `email_rules_master_clean.csv`
2. Test classification accuracy
3. Update Excel reference sheet if needed
4. Document changes in `CHANGELOG.md`
5. Update relevant workflow JSONs

### Documentation Updates
1. Keep `README.md` current with high-level changes
2. Update specific docs in `/docs` for technical changes
3. Maintain `CHANGELOG.md` for all significant updates
4. Update AI context files when adding new features

---

## 📈 Growth Considerations

As the system scales:
1. **Version Control**: Consider semantic versioning for workflow files
2. **Environment Separation**: Maintain dev/staging/production workflow sets
3. **Backup Strategy**: Regular exports of all Make.com scenarios
4. **Monitoring**: Expand audit CSV to track more metrics
5. **Documentation**: Keep architecture docs updated as system evolves

---

**Last Updated**: February 2026  
**Document Version**: 1.0
