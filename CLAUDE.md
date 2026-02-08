# Shipping Automation System

## What This Is
Shipping automation for two e-commerce businesses built on Make.com (formerly Integromat), integrating Monday.com, QuickBooks, and NetParcel carrier APIs.

- **dadsprinting.com** - Direct fulfillment (Standard Shipping)
- **proteamjerseys.com** - Mix of direct and blind shipping (dropshipping)

## Repository Layout

```
blueprints/
  standard/    - 5 Make.com blueprints for direct shipping
  blind/       - 5 Make.com blueprints for blind/dropship shipping
  financial/   - 2 QuickBooks payment verification blueprints
docs/
  api/         - NetParcel API docs and sample JSON requests/responses
  reference/   - Workflow PDFs and reference markdown files
  screenshots/ - Workflow diagram screenshots
  README.md    - Full shipping board & blind shipping guide
  STRUCTURE.md - Detailed repo structure breakdown
  ARCHITECTURE.md - System architecture and data flows
```

## Workflow Sequence
Rate Request → Book Label → Create Invoice → Send Tracking → Mark Fulfilled → QuickBooks Payment Check (every 2 hrs)

## Key Concepts

**Standard Shipping**: Business ships directly to customer. Labels printed at shop via PrintNode. Invoice = cost + $2.50 handling.

**Blind Shipping**: Supplier fulfills order but label shows business name (not supplier). Labels emailed to vendor (not printed). Invoice = cost + 20% markup. Customer never sees supplier info.

## Blueprint Naming
- `Shipping 2.0 *` = Standard shipping
- `BlindShipping 2.0 *` / `Blindshipping 2.0 *` = Blind shipping
- `Invoice Paid *` = Financial integration
- Files are `.blueprint.json` (Make.com scenario exports)

## Integrations
- **Monday.com**: Order management board, status triggers fire webhooks
- **NetParcel API**: UPS, USPS, FedEx rate quotes and label booking (docs in `docs/api/`)
- **QuickBooks**: Invoice creation (OAuth 2.0), payment tracking every 2 hours
- **PrintNode**: Label printing (standard shipping only)
- **Airtable**: Vendor lookup (blind shipping only)
- **Gmail**: Label emails to vendors, error notifications
- **Email/SMS**: Customer tracking notifications

## Business Rules
- Standard: default UPS under 50 lbs, $2.50 handling fee, insurance for orders >$100
- Blind: always use business address as shipper, never reveal supplier, 20% markup
- Invoice created immediately after label generation
- Both financial AND fulfillment status must be complete before order moves to Completed

## Security - Never Commit
- API keys, credentials, webhook URLs
- Customer data (names, addresses, emails)
- Supplier contact information
- Shipping labels, invoice data, tracking numbers

## Working With Blueprints
Blueprint JSON files are Make.com scenario exports. They define automation workflows with modules, connections, and routing logic. When modifying:
1. Read the full blueprint to understand the flow
2. Identify which modules handle the specific logic
3. Make targeted changes to the relevant module configuration
4. Test in Make.com staging before production
