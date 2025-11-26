# Bounded Context Canvas: Nostro Account Management

> **Generated from Legacy System Modernization Pipeline**  
> **BIAN Trinity Classification**: BRAIN  
> **Strategic Classification**: CORE

---

## 1. Name & Description

**Name**: Nostro Account Management

**Description**: Manages the bank's own accounts held at correspondent banks (Nostro accounts). This includes account details, currency, and operational rules like statement generation thresholds.

**Trinity Role**: System of Operation. While it manages master data, its primary BIAN classification is operational, focusing on the rules and administration of these accounts.

---

## 2. Strategic Classification

**Type**: core

✅ **Core Domain** - Differentiating capability, competitive advantage



---

## 3. Business Model

**Revenue Impact**: none

**Cost Center**: cost-reduction

---

## 4. Evolution Stage

**Stage**: custom-built


🔧 Custom-Built - Unique to organization, competitive differentiator



---

## 5. Domain Roles

**Who interacts with this bounded context:**

- **Treasury Operations**: Opens, closes, and manages nostro account details.
- **Settlement Clerk**: References nostro accounts when defining SSIs.

---

## 6. Inbound Communication

**What comes INTO this context:**

- **UI/API**
  - Message: `OpenNostroAccount command`
  - Protocol: sync-rest-api
  - Reason: To create a new nostro account record.

- **Correspondent Relationship Management**
  - Message: `CorrespondentRelationshipEstablished event`
  - Protocol: async-event-kafka
  - Reason: Prerequisite for opening a nostro account with that correspondent.

---

## 7. Outbound Communication

**What goes OUT of this context:**

- **Settlement Instructions Management**
  - Message: `GetNostroAccountDetails query`
  - Protocol: sync-rest-api
  - Reason: To provide valid account details for use in SSIs.

- **SWIFT Gateway**
  - Message: `GenerateMT210 command`
  - Protocol: async-command-queue
  - Reason: Triggered when a statement threshold is breached.

---

## 8. Ubiquitous Language

**Domain-specific terminology:**

**nostro_account**: An account that our bank holds with another bank, denominated in the currency of that country. 'Our money with them'.

**loro_account**: An account that another bank holds with us. 'Their money with us'.

**mt210_statement**: A SWIFT message type for a Notice to Receive, often used for nostro account balance reporting.

**statement_threshold**: A balance limit that, when crossed, triggers an operational event like sending an MT210.

---

## 9. Responsibilities

**What this context does:**

- Maintain the master data for all Nostro and Loro/Vostro accounts.
- Link each account to its host correspondent bank.
- Manage operational rules, such as thresholds for sending MT210 statements.
- Classify accounts by type (Nostro/Loro) and usage (Debit/Credit).
- Provide account details to the Settlement Instructions context.

---

## 10. Aggregates & Entities

**Core domain objects owned by this context:**

### NostroAccount

**Entities**: StatementRule

**Value Objects**: AccountNumber, Currency, AccountType, Money



---

## BIAN v13 Alignment

### Owned BIAN Service Domains


### Correspondent Bank Operations

- **Business Area**: Product & Service
- **Business Domain**: Corporate Banking
- **BIAN URL**: https://bian.org/servicelandscape-13-0-0/object_23.html?object=49493


### BIAN Layer

**Layer**: Business Domain

### Trinity Classification

**Classification**: brain



🧠 **BRAIN** - Operations, Execution, Processing


---

## Related Capabilities

- CAP-002

---

## Key Non-Functional Requirements

- NFR-001
- NFR-008
- NFR-015

---

## Context Relationships

### Upstream Dependencies (This context depends on)

None - This context has no upstream dependencies

### Downstream Consumers (This context provides to)

- **Settlement Instructions Management** (upstream-downstream) - Nostro Accounts (upstream) provides authoritative account data that Settlement Instructions (downstream) consumes.

---

## Legacy System Integration

### Modernization Strategy

**Strategy**: strangler-fig

🌿 **Strangler Fig** - Gradually replace legacy functions over time




### Legacy Artifacts Covered

- KRSTB005
- KRSTB104

### Integration Points

No integration points defined yet.

---

## Notes

Classified as 'BRAIN' by BIAN, but has strong 'MEMORY' characteristics. The design should support both fast data retrieval and the execution of operational rules (e.g., threshold checks).

---

## Context Metadata

- **Context ID**: bc-003
- **Generated**: 2025-11-26T17:29:44.185Z
- **Source**: Legacy System Modernization Pipeline (BIAN v13 + DDD)

---

## Quick Reference Card

| Aspect | Value |
|--------|-------|
| **Name** | Nostro Account Management |
| **Type** | core |
| **Trinity** | brain |
| **BIAN Layer** | Business Domain |
| **Evolution** | custom-built |
| **Aggregates** | 1 |
| **Capabilities** | 1 |
| **NFRs** | 3 |
| **Upstream Deps** | 0 |
| **Downstream Consumers** | 1 |

---

*This Canvas v5 document was auto-generated from the legacy system modernization pipeline. Review and refine as needed with your team.*
