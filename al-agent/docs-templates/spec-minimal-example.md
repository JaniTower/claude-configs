# Feature: Customer Credit Limit

Add credit limit validation to prevent customers from exceeding their allowed credit.

## Requirements

- Add CreditLimit (Decimal) field to Customer table
- Add CreditLimitBlocked (Boolean) field to Customer table
- Validate credit limit is not negative
- Block sales posting if outstanding amount + order exceeds credit limit
- Show credit limit fields on Customer Card
- Show calculated Outstanding Amount on Customer Card

## Object Numbers

| Type | Number | Name |
|------|--------|------|
| Table Extension | 50100 | Customer Credit Limit |
| Codeunit | 50100 | Credit Limit Mgt. |
| Codeunit | 50101 | Credit Limit Integration |
| Page Extension | 50100 | Customer Card Ext |

## Key Procedures

- `CheckCreditLimit(CustomerNo: Code[20])` - Validate customer can post
- `GetOutstandingAmount(CustomerNo: Code[20]): Decimal` - Calculate outstanding
- `IsOverCreditLimit(CustomerNo: Code[20]; OrderAmount: Decimal): Boolean` - Check limit

## Event Subscribers

| Event | Publisher | Calls | Purpose |
|-------|-----------|-------|---------|
| OnBeforePostSalesDoc | Codeunit "Sales-Post" | CheckCreditLimit() | Block posting if over limit |

**Integration Codeunit:** Credit Limit Integration (50101) - Subscribes to Sales-Post and calls Credit Limit Mgt.
