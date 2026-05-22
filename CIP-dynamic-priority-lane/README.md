---
CIP: "?"
Title: Ouroboros Linear Leios - Dynamic Transaction Pricing
Category: Consensus
Status: Proposed
Authors:
  - Will Gould <will.gould@iohk.io>
  - Polina Vinogradova <polina.vinogradova@iohk.io>
  - Nicolas Henin <nicolas.henin@iohk.io>
  - Giorgos Panagiotakos <giorgos.panagiotakos@iohk.io>
Implementors: []
Discussions:
    - ...
Solution-To:
    - CPS-? | Prioritising Urgent Transactions: https://github.com/cardano-foundation/CIPs/pull/1194
Created: YYYY-MM-DD
License: Apache-2.0
---

## Abstract

This CIP defines a dynamic pricing scheme that enables urgency signalling for high urgency transactions at an additional fee cost.

## Motivation: why is this CIP necessary?

See [*CPS-prioritising-urgent-transactions*](https://github.com/cardano-foundation/CIPs/pull/1194).

### Problem

The problem of enabling urgency signalling in a world of linear-Leios becomes much more complex and unintuitive than in Praos. For example, it might seem obvious that the endorser blocks (EBs) could be treated as the non-urgent lane, and ranking blocks (RBs) treated as the priority lane. Practically, however, because of the fact that RBs may contain _either_ an EB certificate _or_ transactions, this split doesn't perform well, as we demonstrate in this CIP.

## Specification

### Glossary

**Standard transaction**: A transaction which is not given priority. Cardano's current transactions.

**Priority transaction**: A transaction which has paid to enter the priority lane. This signals that the transaction should be included before standard transactions, where possible.

**Reserved**: A priority lane mechanism under which block space is reserved for either priority transactions or standard transactions, enforced on-chain.

**Reservation policy - ceiling**: A reservation policy that enforces a maximum on the share of block space occupied by priority transactions. For example, a concrete version of this policy could be that no more than 85% of a block's bytes may carry priority transactions. If priority demand exceeds the cap and standard demand is below the remainder, the cap leaves block space unused.

**Reservation policy - floor**: A reservation policy that reserves a minimum portion of block space exclusively for priority transactions; standard transactions cannot occupy it. For example, with a 15% floor, if no priority transactions exist and standard demand is 100% of block size, only 85% of block space will be occupied.


#### Lanes and routing

**Standard lane**: A pathway for transactions that do not pay the priority fee.

**Priority lane**: A pathway for transactions that do pay priority fee.

**Lane selection (the user-side decision)**: The choice of lane, made by the constructor of a transaction.


#### Pricing primitives

**Pricing coefficient**: The value by which the base fee is multiplied (which results in the quote).

**Quote**: The result of multiplying the pricing coefficient by the base fee; in effect, the dynamic price for a given transaction.

**Priority premium**: The delta between the base fee and the quote.

**Multiplier floor**: The minimum priority-to-standard quote ratio. `multiplier_floor = 16` means priority is always at least 16x the standard per-byte fee.

**Static (pricing)**: Basic Cardano fee, as today.

**Dynamic (pricing)**: EIP-1559 style dynamic fee.

**EIP-1559 (controller)**: todo

**Smoothing window**: todo

**Target utilisation**: todo

**Quote drift**: Potential or true delta between a quote at the time of transaction submission vs the time of inclusion.


#### User-side fee fields

**Posted fee vs actual fee**: todo

**Refund**: The process of returning the unnecessary excess of a fee to a specified address.

**Max fee (max_fee_lovelace / fee ceiling on the user side)**: todo


#### Welfare / actors

**Urgency**: The rate at which the value of a transaction decays.

**Retained value / welfare**: The sum of transaction value that did not decay prior to inclusion.

**Mispriced actor**: todo


### Design Tree: Considered Options

- **One lane** (single-lane EIP-1559) - baseline, no priority signalling
- **Two lanes**
  - **No reservation** (head-of-line priority; shared block-space cap)
    - Static standard, dynamic priority
    - Both dynamic
  - **Priority-floor reservation** (block space that must be used for priority)
    - Static standard, dynamic priority
    - Both dynamic
  - **Priority-ceiling reservation** (cap on priority's share, leaving guaranteed welfare for standard)
    - Static standard, dynamic priority
    - Both dynamic

Two orthogonal axes - **reservation policy** (`none` / `floor` / `ceiling`) and **pricing dynamics** (`static standard + dynamic priority` / `both dynamic`) — give 6 two-lane design points, plus the single-lane baseline.

### Design: Recommended option

### Ledger Changes

### Mempool Design

### Pricing Update Mechanism



## Rationale: how does this CIP achieve its goals?

## Path to Active

### Acceptance Criteria

### Implementation Plan

## Copyright