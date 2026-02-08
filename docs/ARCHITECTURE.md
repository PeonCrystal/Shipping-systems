# System Architecture Overview

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         ORDER SOURCES                               │
├─────────────────────────────────────────────────────────────────────┤
│  - dadsprinting.com (Standard Shipping)                             │
│  - proteamjerseys.com (Blind Shipping / Dropship)                  │
│  - Monday.com Board (Manual Entry)                                  │
└───────────────────────────┬─────────────────────────────────────────┘
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    MONDAY.COM SHIPPING BOARD                        │
├─────────────────────────────────────────────────────────────────────┤
│  - Order details entry (address, dimensions, box count)             │
│  - Status triggers for automation                                   │
│  - Sub-items per box (weight, L x W x H)                           │
│  - Quote review and shipper selection                               │
└───────────────────────────┬─────────────────────────────────────────┘
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    MAKE.COM AUTOMATION ENGINE                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────────────┐  ┌──────────────────────┐                 │
│  │  STANDARD SHIPPING   │  │  BLIND SHIPPING      │                 │
│  ├──────────────────────┤  ├──────────────────────┤                 │
│  │ 1. Rate Request      │  │ 1. Quote Request     │                 │
│  │ 2. Book & Print      │  │ 2. Book & Email      │                 │
│  │ 3. Create Invoice    │  │ 3. Create Invoice    │                 │
│  │ 4. Send Tracking     │  │ 4. Send Tracking     │                 │
│  │ 5. Mark Fulfilled    │  │ 5. Mark Fulfilled    │                 │
│  └──────────────────────┘  └──────────────────────┘                 │
│                                                                      │
│  ┌──────────────────────┐                                           │
│  │  FINANCIAL LOOP      │                                           │
│  ├──────────────────────┤                                           │
│  │ - QB Payment Check   │                                           │
│  │ - Status Updates     │                                           │
│  │ - Runs every 2 hours │                                           │
│  └──────────────────────┘                                           │
│                                                                      │
└───────────────────────────┬─────────────────────────────────────────┘
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    EXTERNAL INTEGRATIONS                             │
├─────────────────────────────────────────────────────────────────────┤
│  - NetParcel API (UPS, USPS, FedEx rate quotes & label booking)    │
│  - QuickBooks (Invoice creation & payment tracking)                 │
│  - PrintNode (Label printing - standard shipping only)              │
│  - Airtable (Vendor lookup - blind shipping only)                   │
│  - Gmail (Label emails to vendors, error notifications)             │
│  - Email/SMS (Customer tracking notifications)                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Standard Shipping Data Flow

```
1. ORDER ENTERED IN MONDAY.COM
   │
   ├─→ Address fields populated (street, city, province, postal, country)
   ├─→ Box count entered → sub-items created
   └─→ Sub-items filled (weight, length, width, height per box)
        │
        ↓
2. STATUS → "Ready for Quotes"
   │
   └─→ Make.com Rate Request Trigger fires
        │
        ├─→ NetParcel API: get quotes from UPS, USPS, FedEx
        └─→ Results written to Google Sheet linked to order
             │
             ↓
3. USER SELECTS SHIPPER (marks "X" on sheet)
   │
   └─→ STATUS → "Book Label"
        │
        ↓
4. Make.com Book & Print Label fires
   │
   ├─→ NetParcel API: book shipment, get label PDF
   ├─→ PrintNode: print label at shop
   ├─→ Monday.com: update tracking number, shipment ID
   └─→ Triggers invoice + tracking workflows
        │
        ├─→ QuickBooks: create invoice (cost + $2.50 handling + insurance)
        └─→ Email/SMS: send tracking to customer
             │
             ↓
5. CARRIER PICKS UP (checked every 4 hours)
   │
   └─→ Make.com Mark Fulfilled fires
        │
        ├─→ Monday.com: status → "Fulfilled" / "Done"
        └─→ Financial loop continues checking payment
```

---

## Blind Shipping Data Flow

```
1. ORDER COMPLETED IN MONDAY.COM
   │
   └─→ STATUS → "Ready to Blind Ship"
        │
        └─→ Blind ship ticket created
             │
             ├─→ Box count entered → sub-items created
             └─→ Airtable: lookup vendor details
                  │
                  ↓
2. STATUS → "Ready for Quotes"
   │
   └─→ Make.com Quote Request fires
        │
        ├─→ NetParcel API: quotes using BUSINESS address as shipper
        └─→ Results written to Google Sheet
             │
             ↓
3. USER SELECTS SHIPPER
   │
   └─→ STATUS → "Book Label"
        │
        ↓
4. Make.com Book & Send Label fires
   │
   ├─→ NetParcel API: book with business as shipper (NOT vendor)
   ├─→ Gmail: email label PDF to vendor (NOT printed locally)
   ├─→ Monday.com: update tracking, shipment ID
   └─→ Triggers invoice + tracking workflows
        │
        ├─→ QuickBooks: invoice with 20% markup (supplier cost hidden)
        └─→ Email/SMS: branded tracking to customer
             │
             ↓
5. CARRIER PICKS UP → Mark Fulfilled (same as standard)
```

---

## Financial & Fulfillment Loop

```
RUNS EVERY 2 HOURS (10 AM, 12 PM, 2 PM, 4 PM)
│
├─→ Scan Monday.com for orders with:
│   - "Shipping unpaid"
│   - "Balance and shipping unpaid"
│   - "Balance owing"
│
├─→ For each order:
│   ├─→ Get shipping invoice number from Monday.com
│   ├─→ Search QuickBooks for payment status
│   │
│   ├─→ IF PAID:
│   │   ├─→ Update Monday.com → "Bills Paid"
│   │   ├─→ IF picked up → move to Completed board
│   │   └─→ IF at shop → update to "Free to Release"
│   │
│   └─→ IF UNPAID:
│       ├─→ Continue monitoring (up to 3 business days)
│       └─→ Trigger customer email reminder
│
└─→ Order moves to Completed ONLY when BOTH:
    - Financial status = Paid
    - Fulfillment status = Fulfilled
```

---

## Integration Points

### NetParcel API
- **Purpose**: Carrier rate quotes and label booking
- **Carriers**: UPS, USPS, FedEx
- **Docs**: `docs/api/`
- **Sample requests/responses**: `docs/api/samples/`

### QuickBooks
- **Auth**: OAuth 2.0
- **Purpose**: Invoice creation and payment tracking
- **Timing**: Invoice created immediately after label; payment checked every 2 hours

### Monday.com
- **Purpose**: Order management board, status triggers
- **Key fields**: Address, dimensions, box count, tracking, shipment ID
- **Triggers**: Status changes fire Make.com webhooks

### PrintNode (Standard only)
- **Purpose**: Print labels at the shop
- **Not used for**: Blind shipping (labels emailed to vendor instead)

### Airtable (Blind only)
- **Purpose**: Vendor lookup and matching

---

## Key Differences: Standard vs Blind

| Aspect | Standard | Blind |
|--------|----------|-------|
| Shipper on label | Business | Business (NOT vendor) |
| Label delivery | Printed at shop (PrintNode) | Emailed to vendor (Gmail) |
| Invoice markup | Cost + $2.50 handling | Cost + 20% markup |
| Vendor visibility | N/A | Hidden from customer |
| Primary business | dadsprinting.com | proteamjerseys.com |

---

**Last Updated**: February 2026
