# Claude Context - Shipping Automation System

This file provides context for AI assistants (like Claude) working with the shipping automation system. It helps AI understand the system architecture, business rules, and common tasks quickly.

## 🎯 System Purpose

This is a **shipping automation system** for two e-commerce businesses:
- **dadsprinting.com** - Direct fulfillment
- **proteamjerseys.com** - Mix of direct and blind shipping (dropshipping)

Built with **Make.com** (formerly Integromat) using 14 automated workflows.

---

## 📂 Repository Structure

```
shipping-automation-system/
├── Standard Shipping (5 blueprints)
│   └── Shipping_2_0_*.json
├── Blind Shipping (5 blueprints)
│   └── BlindShipping_2_0_*.json
└── Financial Integration (2 blueprints)
    └── Invoice_Paid_*.json
```

---

## 🔑 Key Concepts

### Standard Shipping
- Customer orders product in stock
- Business ships directly to customer
- Business name appears on shipping label
- Used primarily by **dadsprinting.com**

### Blind Shipping
- Customer orders product fulfilled by supplier
- Label shows business name (not supplier)
- Protects supplier relationships
- Customer thinks it shipped from the business
- Used primarily by **proteamjerseys.com**

### Workflow Sequence
```
Rate Request → Book Label → Create Invoice → Send Tracking → Mark Fulfilled
                                ↓
                    QuickBooks Payment Verification
```

---

## 🗂️ File Descriptions (Quick Reference)

### Standard Shipping Files

| File | What It Does |
|------|--------------|
| `Shipping_2_0_-_Rate_Request_Trigger` | Gets shipping quotes from carriers |
| `Shipping_2_0_-_Book___Print_Label` | Generates actual shipping labels |
| `Shipping_2_0_Create_Shipping_Invoice` | Creates invoice in QuickBooks |
| `Shipping_2_0_Send_Tracking` | Emails/SMS tracking to customer |
| `Shipping_2_0_Mark_shipments_as_fulfilled` | Updates order status when shipped |

### Blind Shipping Files

| File | What It Does |
|------|--------------|
| `Blindshipping_2_0_-_Label_Quote_Request` | Gets quotes for blind shipments |
| `BlindShipping_2_0_-_Book___Send_Label` | Generates label with business as shipper |
| `BlindShipping_2_0_Create_Shipping_Invoice` | Creates customer invoice (hides supplier cost) |
| `BlindShipping_2_0_Send_Tracking` | Sends branded tracking (no supplier info) |
| `BlindShipping_2_0_Mark_shipments_as_fulfilled` | Updates order (filters supplier details) |

### Financial Files

| File | What It Does |
|------|--------------|
| `Invoice_Paid_-_Quickbooks_System_Check_p1` | Detects payment in QuickBooks |
| `Invoice_Paid_-_Quickbooks_System_Check_p2` | Updates systems after payment |

---

## 🤖 Common AI Assistant Tasks

### Task 1: Explain a Workflow
**User asks**: "What does the Book Label workflow do?"

**Response approach**:
1. Identify if Standard or Blind Shipping
2. Explain the main purpose
3. List key steps in the process
4. Mention integrations (carriers, storage, etc.)
5. Note what triggers it and what it triggers

### Task 2: Troubleshoot Issues
**User asks**: "Labels aren't generating for blind shipments"

**Response approach**:
1. Check BlindShipping Book & Send Label workflow
2. Common issues:
   - Address validation failing
   - Shipper info not properly replaced
   - Carrier API credentials issue
   - Package weight/dimensions format
3. Where to look: Make.com execution logs
4. What to verify: Shipper override configuration

### Task 3: Add New Feature
**User asks**: "I want to add international shipping"

**Response approach**:
1. Identify which workflows need modification
2. New requirements: customs forms, duties, international carriers
3. Suggest creating new blueprints vs modifying existing
4. Point to integration points: Rate Request, Book Label
5. Remind about testing thoroughly

### Task 4: Modify Business Rules
**User asks**: "Change handling fee from $2.50 to $3.50"

**Response approach**:
1. Identify workflow: Create Shipping Invoice (both Standard & Blind)
2. Location: Financial calculation module in Make.com
3. Suggest: Update in data store or scenario variable
4. Remind: Test with sample order
5. Note: Document change in CHANGELOG

---

## 🔧 System Integrations

### Carriers
- **UPS**: OAuth 2.0 authentication
- **USPS**: API Key authentication
- **FedEx**: OAuth 2.0 authentication

### Financial
- **QuickBooks**: OAuth 2.0, invoice creation, payment tracking

### E-commerce
- **Shopify** (implied): Webhooks for order events

### Notifications
- **Email Service**: SendGrid, Mailgun, or similar
- **SMS Service**: Twilio or similar

---

## 📋 Business Rules

### Standard Shipping Rules
1. Use customer's preferred carrier if specified
2. Default to UPS for packages under 50 lbs
3. Add $2.50 handling fee to all orders
4. Include insurance for orders over $100
5. Send tracking within 1 hour of label generation

### Blind Shipping Rules
1. Always use business address as shipper
2. Never reveal supplier information to customer
3. Send separate label instructions to supplier
4. Apply markup to shipping cost (configured per business)
5. Ensure all customer-facing content is branded

### Invoice Rules
1. Create invoice immediately after label generation
2. Include line items for: shipping cost, handling, insurance
3. Calculate tax based on customer location
4. Set payment terms (configured per business)
5. Trigger payment verification workflow

---

## 🚨 Important Warnings

### Never Expose in Customer-Facing Content
- Supplier names or addresses
- Actual supplier shipping costs
- Internal cost markups
- Supplier account numbers
- Dropship relationships

### Never Commit to GitHub
- API keys and credentials
- Customer data (names, addresses, emails)
- Supplier contact information
- Shipping labels (PDFs)
- Invoice data
- Tracking numbers in bulk
- Production webhook URLs

### Always Test Before Production
- Address validation logic
- Shipper information replacement (blind shipping)
- Invoice cost calculations
- Notification delivery
- Error handling and retries

---

## 💡 Quick Problem Solving

### "Workflow isn't triggering"
1. Check webhook configuration in e-commerce platform
2. Verify webhook URL is correct in Make.com
3. Test with manual trigger
4. Check Make.com scenario is active
5. Review execution history for errors

### "Label has wrong shipper"
1. Confirm using correct workflow (Standard vs Blind)
2. Check shipper override configuration
3. Verify business address in data store
4. Test address replacement logic
5. Review PDF generation settings

### "Invoice amount incorrect"
1. Verify carrier API returned correct cost
2. Check handling fee configuration
3. Confirm tax calculation rules
4. Review markup settings (blind shipping)
5. Test with sample order end-to-end

### "Customer didn't receive tracking"
1. Check email/SMS delivery logs
2. Verify customer contact info in order
3. Confirm notification template is correct
4. Check email service API status
5. Look for bounces or delivery failures

---

## 🎓 Learning Resources

### Understanding the System
1. Start with README.md for overview
2. Read ARCHITECTURE.md for data flow
3. Review STRUCTURE.md for file details
4. Follow SETUP.md for configuration

### Make.com Resources
- Make.com documentation: docs.make.com
- Scenario execution logs: Primary debugging tool
- Webhook testing: Use Make.com's webhook tools
- Data stores: Store configuration variables

### Carrier APIs
- UPS Developer Portal: developer.ups.com
- USPS Web Tools: usps.com/business/web-tools-apis
- FedEx Developer: developer.fedex.com

---

## 🔍 Code Patterns to Look For

### Scenario Naming Conventions
- `Shipping_2_0_` = Standard shipping workflows
- `BlindShipping_2_0_` = Blind shipping workflows
- `Invoice_Paid_` = Financial integration workflows

### Data Flow Patterns
```
Webhook → Validate → API Call → Transform → Store → Notify
```

### Error Handling Patterns
```
Try API Call → If Fail → Retry (3x) → If Still Fail → Log & Alert
```

### Blind Shipping Pattern
```
Order → Replace Shipper Info → Generate Label → Send to Supplier & System
```

---

## 📝 Common Modifications

### Adding a New Carrier
1. Get API credentials
2. Add carrier module to Rate Request workflow
3. Add carrier module to Book Label workflow
4. Configure fallback logic
5. Test rate quotes and label generation

### Changing Notification Templates
1. Identify notification workflow (Send Tracking)
2. Locate email/SMS template module
3. Update content (maintain branding)
4. Test with sample order
5. Verify formatting on mobile devices

### Adjusting Pricing Rules
1. Locate Create Invoice workflow
2. Find calculation module
3. Update handling fee or markup percentage
4. Test calculation with various order amounts
5. Document change

### Adding New Business
1. Determine if Standard or Blind shipping needed
2. Clone appropriate workflows
3. Update business name and address
4. Configure carrier accounts
5. Set up separate webhooks
6. Test complete flow

---

## 🎯 Success Metrics

### System Health Indicators
- Label generation success rate > 98%
- Average label generation time < 5 seconds
- Invoice creation success rate > 99%
- Tracking notification delivery > 95%
- Error rate < 2%

### Business Metrics
- Orders processed per day
- Shipping cost accuracy (quoted vs actual)
- Customer satisfaction with shipping communication
- Carrier performance comparison
- Blind shipping relationship integrity

---

## 🤝 Working with This System

### Best Practices
1. Always read the full workflow before modifying
2. Test in Make.com staging environment first
3. Document all changes in CHANGELOG.md
4. Keep credentials secure (never in code)
5. Monitor execution logs after changes
6. Maintain separation between Standard and Blind workflows

### When Helping Users
1. Ask which business (dadsprinting.com or proteamjerseys.com)
2. Ask which workflow type (Standard or Blind)
3. Request specific error messages from Make.com logs
4. Suggest testing with sample order
5. Remind about documentation updates

### Communication Style
- Be specific about file names (include full path)
- Reference Make.com module names when relevant
- Provide step-by-step instructions for changes
- Warn about potential impacts (blind shipping privacy, etc.)
- Link to relevant documentation sections

---

## 🆘 Emergency Procedures

### System Down
1. Check Make.com service status
2. Verify carrier API status
3. Review recent scenario changes
4. Switch to manual order processing if needed
5. Document issue for post-mortem

### Data Breach Concern
1. Immediately deactivate affected scenarios
2. Rotate API credentials
3. Audit data access logs
4. Notify affected parties if required
5. Review and strengthen security

### Blind Shipping Privacy Breach
1. Identify which orders were affected
2. Verify what information was exposed
3. Contact affected customers if needed
4. Fix shipper override logic immediately
5. Add additional validation checks

---

**Context Version**: 1.0  
**Last Updated**: February 2026  
**System Version**: 2.0

---

## 💬 Quick Reference Commands

When user asks about:
- **"How does X work?"** → Explain workflow + integrations + data flow
- **"X is broken"** → Ask for Make.com logs + check common issues
- **"Add feature Y"** → Identify affected workflows + integration points + testing needs
- **"Change setting Z"** → Locate in workflow + explain impact + testing steps
- **"Standard vs Blind?"** → Explain difference + use cases + privacy implications

Remember: This system handles **real customer orders** and **supplier relationships**. Privacy and accuracy are critical!
