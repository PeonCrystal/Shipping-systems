# Email Triage System

An automated email management and customer service system built with Make.com (formerly Integromat) that intelligently routes, categorizes, and responds to customer inquiries.

## 📋 Overview

This system automates email triage and customer service workflows, integrating with various business tools to provide intelligent routing, automated responses, and comprehensive tracking of customer communications.

## 🗂️ Repository Structure

```
emailtriagesystem/
├── docs/                          # System documentation
│   ├── readme.md                  # Docs directory documentation
│   ├── change_log.md.rtf         # System change history
│   ├── failure_modes.rtf         # Known failure scenarios
│   ├── label_state_machine.rtf   # Email labeling logic
│   └── system_architecture.d.rtf # Architecture documentation
│
├── rules/                         # Email classification rules
│   ├── readme.md
│   ├── email_rules_master_clean.csv
│   └── Email Tags W_Associated Actions (1).xlsx
│
├── workflows/                     # Make.com automation scenarios
│   ├── CATCH-ALL - Missed Email Recovery.json
│   ├── COMMS - Following Up Reply Automator.json
│   ├── Comms Agent - Customer Service Inquiry -_Correct Team Member (1).json
│   ├── Comms Agent - Needs Info to Quote.json
│   ├── Comms Agent - Quote Rejected.json
│   ├── Comms Agent - State 3 - FAQ Bot (1).json
│   ├── Contact Us Form (Website) to Pipedrive Uncontacted (2).json
│   ├── Contact Us Form - Autoresponder & Router - COMMS (2).json
│   ├── EMAIL TRIAGE SYSTEM - - TIGHTEN.json
│   ├── Hourly Form Email Cleaner.json
│   ├── Invoice Paid Auto Forwarder (1).json
│   ├── Label Logic Controller (1).json
│   ├── TAG-SYS Label Cleanup (Daily) (1).json
│   └── readme.md
│
├── AI TRIAGE AUDIT - Sheet1.csv   # System performance audit
├── CHANGELOG.md                   # Recent changes and updates
├── CLAUDE_CONTEXT.md              # AI assistant context/prompts
├── CONTRIBUTING.md                # Contribution guidelines
├── Claude_context.md              # Additional AI context
├── RULES.md                       # Business rules documentation
├── Rules                          # Rules reference file
├── SETUP.md                       # System setup instructions
├── clinerules.md                  # CLI/terminal rules
├── gitignore                      # Git ignore patterns
└── read_me.rtf                    # Original readme
```

## 🚀 Key Components

### 1. Email Triage System
The core automation that:
- Monitors incoming emails across multiple channels
- Classifies emails based on content, sender, and context
- Routes to appropriate team members or automated responses
- Tracks email state through label management

### 2. Automated Response Agents
Specialized bots for different scenarios:
- **FAQ Bot**: Handles common questions automatically
- **Quote Request Handler**: Routes quote inquiries to sales
- **Follow-up Automator**: Ensures timely responses
- **Customer Service Router**: Directs to correct team member

### 3. Integration Points
- **Gmail**: Primary email interface
- **Pipedrive CRM**: Customer relationship management
- **Make.com**: Workflow automation platform
- **Contact Forms**: Website inquiry capture

### 4. Recovery & Monitoring
- **Catch-All Recovery**: Rescues missed emails
- **Label Cleanup**: Daily maintenance tasks
- **Hourly Form Cleaner**: Spam/invalid form submission management
- **Audit System**: Performance tracking and optimization

## 📊 Email Classification Rules

The system uses a master rule set (`email_rules_master_clean.csv`) that defines:
- Email categories and tags
- Associated automated actions
- Routing destinations
- Priority levels

See the `/rules` directory for detailed classification logic.

## 🔧 Setup & Configuration

Detailed setup instructions can be found in `SETUP.md`, including:
- Make.com scenario configuration
- Gmail label structure
- Pipedrive integration
- Environment variables and API keys

## 📝 Documentation

### Core Documentation
- **SETUP.md**: Initial system configuration
- **RULES.md**: Business logic and decision trees
- **CONTRIBUTING.md**: How to modify and extend the system
- **CHANGELOG.md**: Version history and updates

### Technical Documentation (`/docs`)
- **system_architecture.d.rtf**: System design and data flow
- **label_state_machine.rtf**: Email state management
- **failure_modes.rtf**: Error handling and recovery
- **change_log.md.rtf**: Historical changes

## 🤖 AI Assistant Integration

This system includes AI assistant context files (`CLAUDE_CONTEXT.md`) that help AI tools understand:
- System architecture
- Business rules
- Common tasks and modifications
- Troubleshooting procedures

## 🔍 Monitoring & Maintenance

### Regular Maintenance Tasks
1. **Daily**: Label cleanup automation runs
2. **Hourly**: Form email cleaning
3. **Weekly**: Review audit logs
4. **Monthly**: Update email classification rules

### Performance Monitoring
Check `AI TRIAGE AUDIT - Sheet1.csv` for:
- Email processing success rates
- Response time metrics
- Classification accuracy
- Failure patterns

## 🚨 Troubleshooting

Common issues and solutions documented in `/docs/failure_modes.rtf`

## 📧 Contact & Support

For questions about this system, contact the business operations team.

## 📄 License

Proprietary - Internal business system for dadsprinting.com and proteamjerseys.com

---

**Last Updated**: February 2026  
**Maintained By**: PeonCrystal  
**Repository**: emailtriagesystem
