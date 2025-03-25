# BitLend Protocol - Technical Documentation

## Overview

A non-custodial lending system enabling BTC-backed loans through Stacks L2 smart contracts. Implements decentralized risk management with Bitcoin-finalized settlements.

## Key Features

1. **BTC Collateralization** - Native support for Bitcoin-pegged assets
2. **Dynamic Risk Parameters** - Programmatic collateral ratios & liquidation thresholds
3. **Automated Interest Accrual** - Block-based interest calculations
4. **Decentralized Governance** - Owner-controlled risk parameters
5. **Liquidation Engine** - Price-triggered position closures

## Technical Specifications

### Constants

- `CONTRACT_OWNER`: Deployer address
- `VALID-ASSETS`: ["BTC", "STX"] (Supported collateral types)
- Error Codes: Standardized error reporting (100-111 series)

### Data Variables

- `platform-initialized`: Contract activation status
- `minimum-collateral-ratio`: 150% initial requirement
- `liquidation-threshold`: 120% trigger level
- `platform-fee-rate`: 1% protocol fee
- `total-btc-locked`: Aggregate collateral
- `total-loans-issued`: Loan counter

### Data Maps

1. **loans**: Loan metadata storage

   - Borrower address
   - Collateral amount
   - Loan amount
   - Interest rate
   - Block timestamps
   - Status tracking (active/repaid/liquidated)

2. **user-loans**: User-loan index (10 loans/user cap)
3. **collateral-prices**: Oracle price feed storage

## Core Functionalities

### 1. Platform Management

- `initialize-platform`: Activates contract (owner-only)
- Security: Prevents re-initialization

### 2. Lending Operations

- `deposit-collateral`: Lock BTC collateral

  - Updates `total-btc-locked`
  - Requires non-zero amounts

- `request-loan`: Create new loan position

  - Validates collateral ratio
  - Generates unique loan ID
  - Tracks block height for interest

- `repay-loan`: Close loan position
  - Calculates accrued interest
  - Updates collateral reserves
  - Manages loan status transitions

### 3. Risk Management

- Automated collateral checks via `check-liquidation`
- Price-triggered `liquidate-position` execution
- Private functions:
  - `calculate-collateral-ratio`: Real-time risk assessment
  - `calculate-interest`: Block-based accrual

### 4. Governance Functions

- `update-collateral-ratio`: Adjust minimum ratio (≥110%)
- `update-liquidation-threshold`: Modify trigger level (≥110%)
- `update-price-feed`: Oracle management (BTC/STX)

### 5. Read Functions

- `get-loan-details`: Full loan metadata
- `get-user-loans`: User position overview
- `get-platform-stats`: Protocol analytics
- `get-valid-assets`: Supported collateral list

## Workflow

### User Flow

1. Admin initializes platform
2. Oracle updates BTC price feed
3. User deposits collateral
4. Loan request with BTC locking
5. Periodic interest accrual
6. Repayment or liquidation execution

### Liquidation Process

1. Price update triggers ratio check
2. Threshold breach detected
3. Position marked liquidated
4. Collateral seized
5. User loan record purged

## Security Model

### 1. Bitcoin Integration

- Stacks L2 finality with Bitcoin settlements
- sBTC/wBTC compatibility layer

### 2. Risk Controls

- Over-collateralization requirements
- Price feed validity checks
- Loan:collateral ratio safeguards

### 3. Access Controls

- Owner-restricted functions
- Borrower-specific operations
- Oracle authentication

### 4. Error Handling

- Input validation guards
- State transition checks
- Custom error codes (100-111 series)

## Testing

### Test Cases

1. Contract initialization permissions
2. Collateral ratio enforcement
3. Interest calculation accuracy
4. Liquidation trigger validation
5. Edge case handling:
   - Zero-value transactions
   - Invalid loan IDs
   - Price feed manipulation attempts

## Installation

### Requirements

- Clarinet SDK
- Stacks Testnet access
- Bitcoin testnet node

```bash
git clone https://github.com/sarah-osam/bitlend
clarinet console

```

## References

1. Stacks Documentation
2. Clarity Language Reference
3. Bitcoin Improvement Proposals (BIPs)
