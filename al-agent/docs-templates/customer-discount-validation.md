# Feature: Customer Discount Validation

Validate and apply customer discounts based on customer type (Regular or VIP).

## Overview

Implement a discount system that automatically calculates and validates discounts based on customer type. Regular customers receive up to 5% discount, VIP customers receive up to 10% discount. The system prevents users from entering discounts that exceed the allowed maximum.

## Functional Requirements

### FR-1: Customer Type Field
- Add Customer Type enum field to Customer table
- Values: Regular (default), VIP
- If blank, defaults to "Regular"

### FR-2: Discount Calculation
- Regular customers: 5% discount
- VIP customers: 10% discount
- Apply discount automatically on sales orders

### FR-3: Discount Validation
- Validate manually entered discounts against maximum allowed
- Regular customers: Maximum 5% discount
- VIP customers: Maximum 10% discount
- Show error and prevent saving if discount exceeds maximum

### FR-4: Business Rules
- Customer type must be validated before discount calculation
- Discount percentage must be between 0 and maximum allowed
- Order amount must be positive

## Technical Design

### Objects

| Type | Number | Name | Description |
|------|--------|------|-------------|
| Enum | 50100 | Customer Type | Regular, VIP values |
| Table Extension | 50100 | Customer Discount Ext | Adds Customer Type to Customer |
| Codeunit | 50100 | Discount Mgt. | Discount calculation and validation |
| Codeunit | 50101 | Discount Integration | Event subscribers for sales line processing |
| Page Extension | 50100 | Customer Card Ext | Shows Customer Type field |

### Key Procedures

```
Discount Mgt. (Codeunit 50100)
├── GetDiscountPercentage(CustomerNo: Code[20]): Decimal
│   └── Returns discount % based on customer type
├── GetMaxDiscountPercentage(CustomerNo: Code[20]): Decimal
│   └── Returns maximum allowed discount for customer type
├── ValidateDiscount(CustomerNo: Code[20]; DiscountPct: Decimal): Boolean
│   └── Validates discount doesn't exceed maximum
└── ApplyDiscount(var SalesLine: Record "Sales Line")
    └── Applies calculated discount to sales line
```

### Event Subscribers

| Event | Publisher | Calls | Purpose |
|-------|-----------|-------|---------|
| OnAfterInsertEvent | Table "Sales Line" | ApplyDiscount() | Auto-apply discount on new lines |
| OnBeforeValidateEvent (Line Discount %) | Table "Sales Line" | ValidateDiscount() | Validate manual discount entry |

**Integration Codeunit:** Discount Integration (50101) - Subscribes to Sales Line events and calls Discount Mgt.

### Data Flow

```
Sales Order Entry
       ↓
Get Customer Type → Discount Mgt.
       ↓
Calculate/Validate Discount
       ↓
Apply to Sales Line (or Error if exceeds max)
```

## Non-Functional Requirements

### Performance
- Use SetLoadFields when reading Customer record

### Extensibility
- Public procedures for customization by other extensions

## Edge Cases

| Scenario | Expected Behavior |
|----------|-------------------|
| Customer Type is blank | Default to "Regular" (5% max) |
| Negative discount entered | Error: Discount must be >= 0 |
| Discount = 0 | Valid, no discount applied |
| Order amount is 0 or negative | Skip discount calculation |

## Out of Scope

- Quantity-based discounts
- Date-based promotional discounts
- Discount approval workflow
