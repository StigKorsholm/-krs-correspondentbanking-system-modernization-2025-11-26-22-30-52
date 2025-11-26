# Bounded Context Canvas: Settlement Instructions Management

> **Generated from Legacy System Modernization Pipeline**  
> **BIAN Trinity Classification**: MEMORY  
> **Strategic Classification**: CORE

---

## 1. Name & Description

**Name**: Settlement Instructions Management

**Description**: Manages Standard Settlement Instructions (SSIs), which are the definitive rules for routing payments. This context is the authoritative source for 'how' to pay a counterparty in a specific currency.

**Trinity Role**: System of Record. Provides a highly available, low-latency repository of routing and settlement reference data.

---

## 2. Strategic Classification

**Type**: core

✅ **Core Domain** - Differentiating capability, competitive advantage



---

## 3. Business Model

**Revenue Impact**: indirect

**Cost Center**: technology-platform

---

## 4. Evolution Stage

**Stage**: product



📦 Product - Standardized, configurable, proven solutions


---

## 5. Domain Roles

**Who interacts with this bounded context:**

- **Settlement Clerk**: Creates, updates, and validates SSIs for counterparties.
- **Payment Engine**: Queries for the active SSI for a given payment instruction.
- **Treasury System**: Consumes SSI data to forecast liquidity needs.

---

## 6. Inbound Communication

**What comes INTO this context:**

- **Correspondent Relationship Management**
  - Message: `CorrespondentRelationshipEstablished event`
  - Protocol: async-event-kafka
  - Reason: To enable the creation of SSIs for the new correspondent.

- **UI/API**
  - Message: `CreateSSI command`
  - Protocol: sync-rest-api
  - Reason: User action to define a new settlement instruction.

---

## 7. Outbound Communication

**What goes OUT of this context:**

- **Payment Engine**
  - Message: `GetActiveSSIForPayment query`
  - Protocol: sync-rest-api
  - Reason: To retrieve the correct routing for a live payment.

- **Correspondent Data Gateway**
  - Message: `SSIUpdated event`
  - Protocol: async-event-kafka
  - Reason: To allow the gateway to update its caches or data exports.

---

## 8. Ubiquitous Language

**Domain-specific terminology:**

**standard_settlement_instruction_ssi**: A standing instruction that specifies a party's preferred method for receiving payments.

**routing_path**: The chain of correspondent banks (Pay-To, Pay-Through) required to complete a payment.

**clearing_system**: A financial infrastructure (e.g., TARGET2, Fedwire) that settles transactions.

**exception_routing**: A non-standard routing path used for specific scenarios or counterparties.

**vostro_account**: An account held by a correspondent bank at our bank. From our perspective, it's a 'Vostro'.

---

## 9. Responsibilities

**What this context does:**

- Maintain the central registry of all Standard Settlement Instructions.
- Define and manage standard and exception-based payment routing paths.
- Link SSIs to specific counterparties, currencies, and products.
- Provide low-latency read access to SSI data for payment engines.
- Manage the data required for both legacy (MT) and modern (MX) message formats.
- Ensure the integrity and validity of routing chains.

---

## 10. Aggregates & Entities

**Core domain objects owned by this context:**

### StandardSettlementInstruction

**Entities**: RoutingPath, IntermediaryBank, BeneficiaryBank

**Value Objects**: ClearingSystemCode, PayToAccount, InFavorOf



---

## BIAN v13 Alignment

### Owned BIAN Service Domains


### Party Routing Profile

- **Business Area**: Reference Data
- **Business Domain**: Party Management
- **BIAN URL**: https://bian.org/servicelandscape-13-0-0/object_23.html?object=49495


### BIAN Layer

**Layer**: Reference Data

### Trinity Classification

**Classification**: memory

💾 **MEMORY** - System of Record, Reference Data




---

## Related Capabilities

- CAP-003

---

## Key Non-Functional Requirements

- NFR-005
- NFR-008
- NFR-013
- NFR-016

---

## Context Relationships

### Upstream Dependencies (This context depends on)

- **Correspondent Relationship Management** (upstream-downstream) - Relationship Management (upstream) provides authoritative correspondent data that Settlement Instructions (downstream) consumes.
- **Nostro Account Management** (upstream-downstream) - Nostro Accounts (upstream) provides authoritative account data that Settlement Instructions (downstream) consumes.
- **Clearing System Directory** (upstream-downstream) - The Directory (upstream) provides reference data about clearing systems used to validate and build SSIs (downstream).

### Downstream Consumers (This context provides to)

None - This context has no downstream consumers

---

## Legacy System Integration

### Modernization Strategy

**Strategy**: parallel-run


⚖️ **Parallel Run** - Run old and new side-by-side, compare results



### Legacy Artifacts Covered

- KRSTB045
- KRSTB047
- KRSTB054
- KRSTB056

### Integration Points

No integration points defined yet.

---

## Notes

This is a classic 'MEMORY' context. CQRS is a highly recommended pattern to optimize for high-read, low-write scenarios. Caching is critical for meeting performance NFRs.

---

## Context Metadata

- **Context ID**: bc-002
- **Generated**: 2025-11-26T17:29:44.185Z
- **Source**: Legacy System Modernization Pipeline (BIAN v13 + DDD)

---

## Quick Reference Card

| Aspect | Value |
|--------|-------|
| **Name** | Settlement Instructions Management |
| **Type** | core |
| **Trinity** | memory |
| **BIAN Layer** | Reference Data |
| **Evolution** | product |
| **Aggregates** | 1 |
| **Capabilities** | 1 |
| **NFRs** | 4 |
| **Upstream Deps** | 3 |
| **Downstream Consumers** | 0 |

---

*This Canvas v5 document was auto-generated from the legacy system modernization pipeline. Review and refine as needed with your team.*
