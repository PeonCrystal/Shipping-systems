# Repository Structure Guide

This document provides a detailed breakdown of the Shipping Automation System repository structure and the purpose of each component.

## Directory Structure

```
Shipping-systems/
├── CLAUDE.md                          # Claude Code project context
├── README.md                          # Main repository overview
├── CLAUDE_CONTEXT.md                  # Detailed AI assistant context
│
├── blueprints/                        # Make.com automation blueprints
│   ├── standard/                      # Standard (direct) shipping workflows
│   │   ├── Shipping 2.0 - Rate Request Trigger.blueprint.json
│   │   ├── Shipping 2.0 - Book & Print Label.blueprint.json
│   │   ├── Shipping 2.0 Create Shipping Invoice based off cost.blueprint.json
│   │   ├── Shipping 2.0 Send Tracking.blueprint.json
│   │   └── Shipping 2.0 Mark shipments as fulfilled when picked up.blueprint.json
│   │
│   ├── blind/                         # Blind shipping (dropship) workflows
│   │   ├── Blindshipping 2.0 - Label Quote Request.blueprint.json
│   │   ├── BlindShipping 2.0 - Book & Send Label.blueprint.json
│   │   ├── BlindShipping 2.0 Create Shipping Invoice based off cost.blueprint.json
│   │   ├── BlindShipping 2.0 Send Tracking.blueprint.json
│   │   └── BlindShipping 2.0 Mark shipments as fulfilled when picked up.blueprint.json
│   │
│   └── financial/                     # Financial integration workflows
│       ├── Invoice Paid - Quickbooks System Check p1.blueprint.json
│       └── Invoice Paid - Quickbooks System Check p2.json
│
└── docs/                              # Documentation
    ├── README.md                      # Main shipping board & blind shipping guide
    ├── STRUCTURE.md                   # This file
    ├── ARCHITECTURE.md                # System architecture and data flow
    ├── LICENSE                        # License file
    │
    ├── api/                           # API documentation
    │   ├── Netparcel API Documentation *.md
    │   ├── netParcel_JSON_API_Developer_Guide_v1.4.pdf
    │   └── samples/                   # Sample API request/response JSON
    │       ├── sampleJSONOrderRequest.txt
    │       ├── sampleJSONOrderResponse.txt
    │       ├── sampleJSONRatingRequest.txt
    │       ├── sampleJSONRatingResponse.txt
    │       ├── sampleJSONShipRequest.txt
    │       ├── sampleJSONShipResponse.txt
    │       ├── sampleJSONCancelShipmentRequest.txt
    │       └── sampleJSONCancelShipmentResponse.txt
    │
    ├── reference/                     # Reference PDFs and workflow documentation
    │   ├── Shipping workflow PDFs (9 files)
    │   ├── @Shipping - Book & Print Label *.md
    │   ├── Automations for Shipping & Blindshipping Board *.md
    │   └── automations-guide.md
    │
    └── screenshots/                   # Workflow diagram screenshots
        ├── Screenshot_2024-12-26_at_10.53.10_PM.png
        ├── Screenshot_2024-12-26_at_2.02.31_PM.png
        └── Screenshot_2024-12-26_at_5.50.55_PM.png
```

## Blueprint Categories

### Standard Shipping (`blueprints/standard/`)
Direct fulfillment workflows where the business ships to the customer. Used primarily by **dadsprinting.com**.

| Blueprint | Purpose |
|-----------|---------|
| Rate Request Trigger | Gets shipping quotes from carriers via NetParcel API |
| Book & Print Label | Generates and prints shipping labels |
| Create Shipping Invoice | Creates invoice in QuickBooks with costs + $2.50 handling |
| Send Tracking | Emails/SMS tracking info to customer |
| Mark Fulfilled | Updates order status when carrier picks up |

### Blind Shipping (`blueprints/blind/`)
Dropship workflows where a supplier fulfills but the business name appears on labels. Used primarily by **proteamjerseys.com**.

| Blueprint | Purpose |
|-----------|---------|
| Label Quote Request | Gets quotes with business address as shipper |
| Book & Send Label | Generates label and emails to vendor (not printed locally) |
| Create Shipping Invoice | Creates customer invoice with 20% markup (hides supplier cost) |
| Send Tracking | Sends branded tracking (no supplier info exposed) |
| Mark Fulfilled | Updates order status, filters supplier details |

### Financial Integration (`blueprints/financial/`)
QuickBooks payment verification workflows that run on schedule.

| Blueprint | Purpose |
|-----------|---------|
| Quickbooks System Check p1 | Detects payments in QuickBooks every 2 hours |
| Quickbooks System Check p2 | Updates Monday.com statuses after payment confirmed |

## Workflow Sequence

```
Rate Request → Book Label → Create Invoice → Send Tracking → Mark Fulfilled
                                ↓
                    QuickBooks Payment Verification (every 2 hours)
```

## File Naming Conventions

- `Shipping 2.0 *` = Standard shipping workflows
- `BlindShipping 2.0 *` / `Blindshipping 2.0 *` = Blind shipping workflows
- `Invoice Paid *` = Financial integration workflows
- `.blueprint.json` = Make.com scenario export files

## Adding New Workflows

1. Create and test the workflow in Make.com
2. Export as JSON blueprint
3. Place in the appropriate `blueprints/` subdirectory
4. Update this STRUCTURE.md document
5. Update CLAUDE_CONTEXT.md if it changes system behavior

## Sensitive Information

The following must **never** be committed:
- API keys and credentials
- Customer data (names, addresses, emails)
- Supplier contact information
- Shipping labels (PDFs)
- Invoice data
- Tracking numbers in bulk
- Production webhook URLs

---

**Last Updated**: February 2026
