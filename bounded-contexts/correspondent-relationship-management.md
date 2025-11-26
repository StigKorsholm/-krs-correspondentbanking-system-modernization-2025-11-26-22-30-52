# Bounded Context Canvas: Correspondent Relationship Management

> **Generated from Legacy System Modernization Pipeline**  
> **BIAN Trinity Classification**: HEART  
> **Strategic Classification**: CORE

---

## 1. Name & Description

**Name**: Correspondent Relationship Management

**Description**: Manages the complete lifecycle of correspondent banking relationships, including master data for bank entities, and the authorization rules (RMA) governing message exchange. Acts as the 'HEART' of the correspondent banking domain.

**Trinity Role**: System of Agreement. Manages the stateful lifecycle of inter-bank relationships and their associated business rules and authorizations.

---

## 2. Strategic Classification

**Type**: core

✅ **Core Domain** - Differentiating capability, competitive advantage



---

## 3. Business Model

**Revenue Impact**: indirect

**Cost Center**: cost-reduction

---

## 4. Evolution Stage

**Stage**: custom-built


🔧 Custom-Built - Unique to organization, competitive differentiator



---

## 5. Domain Roles

**Who interacts with this bounded context:**

- **Relationship Manager**: Onboards new correspondents and maintains relationship data.
- **Compliance Officer**: Performs KYC/AML checks and approves new relationships.
- **SWIFT Administrator**: Configures and manages RMA authorizations.

---

## 6. Inbound Communication

**What comes INTO this context:**

- **Onboarding System**
  - Message: `OnboardCorrespondent command`
  - Protocol: sync-rest-api
  - Reason: To initiate the creation of a new correspondent relationship.

- **SWIFT Network**
  - Message: `RMA Authorization Update message`
  - Protocol: async-swift-message
  - Reason: To reflect changes in RMA initiated by the counterparty.

---

## 7. Outbound Communication

**What goes OUT of this context:**

- **All Contexts**
  - Message: `CorrespondentRelationshipEstablished event`
  - Protocol: async-event-kafka
  - Reason: To notify downstream systems that a new correspondent is available for use.

- **Settlement Instructions Management**
  - Message: `GetCorrespondentDetails query`
  - Protocol: sync-rest-api
  - Reason: To provide validated correspondent data for creating SSIs.

---

## 8. Ubiquitous Language

**Domain-specific terminology:**

**correspondent_bank**: A financial institution providing services on behalf of another financial institution.

**bic**: Bank Identifier Code (ISO 9362), the unique address for a financial institution in the SWIFT network.

**lei**: Legal Entity Identifier (ISO 17442), a global identifier for legal entities participating in financial transactions.

**rma**: Relationship Management Application. A SWIFT standard for managing which message types can be exchanged between two correspondents.

**relationship_agreement**: The set of rules, including RMA, that governs the operational relationship with a correspondent.

**national_central_bank**: The central bank of a country, often a key correspondent for domestic currency settlement.

---

## 9. Responsibilities

**What this context does:**

- Maintain the master record for each Correspondent Bank entity.
- Validate BIC and LEI formats and checksums upon entry.
- Manage the lifecycle of the relationship (e.g., Active, Suspended, Terminated).
- Administer SWIFT RMA permissions for sending and receiving specific MT/MX messages.
- Provide an immutable audit trail for all changes to relationship data and authorizations.
- Publish events when a correspondent is created or its status changes.

---

## 10. Aggregates & Entities

**Core domain objects owned by this context:**

### CorrespondentBank

**Entities**: BIC, LEI, NationalCentralBankLink

**Value Objects**: BankIdentifier, Address, LegalEntityName


### RelationshipManagementAgreement

**Entities**: AuthorizedMessage

**Value Objects**: MessageServiceType, MessageType, Direction



---

## BIAN v13 Alignment

### Owned BIAN Service Domains


### Correspondent Bank Relationship

- **Business Area**: Product & Service
- **Business Domain**: Corporate Banking
- **BIAN URL**: https://bian.org/servicelandscape-13-0-0/object_23.html?object=49492


### BIAN Layer

**Layer**: Business Domain

### Trinity Classification

**Classification**: heart


💚 **HEART** - Lifecycle Management, Relationships, Agreements



---

## Related Capabilities

- CAP-001
- CAP-004

---

## Key Non-Functional Requirements

- NFR-001
- NFR-002
- NFR-003
- NFR-006
- NFR-008

---

## Context Relationships

### Upstream Dependencies (This context depends on)

None - This context has no upstream dependencies

### Downstream Consumers (This context provides to)

- **Settlement Instructions Management** (upstream-downstream) - Relationship Management (upstream) provides authoritative correspondent data that Settlement Instructions (downstream) consumes.

---

## Legacy System Integration

### Modernization Strategy

**Strategy**: strangler-fig

🌿 **Strangler Fig** - Gradually replace legacy functions over time




### Legacy Artifacts Covered

- KRSTB059
- KRSTB060
- KRSTB103
- KRSTB118

### Integration Points

No integration points defined yet.

---

## Notes

This is a core 'HEART' context. Focus on ACID transactions, workflow management, and robust auditing. It provides the foundational 'who' and 'what is allowed' for all other contexts.

---

## Context Metadata

- **Context ID**: bc-001
- **Generated**: 2025-11-26T17:29:44.185Z
- **Source**: Legacy System Modernization Pipeline (BIAN v13 + DDD)

---

## Quick Reference Card

| Aspect | Value |
|--------|-------|
| **Name** | Correspondent Relationship Management |
| **Type** | core |
| **Trinity** | heart |
| **BIAN Layer** | Business Domain |
| **Evolution** | custom-built |
| **Aggregates** | 2 |
| **Capabilities** | 2 |
| **NFRs** | 5 |
| **Upstream Deps** | 0 |
| **Downstream Consumers** | 1 |

---

*This Canvas v5 document was auto-generated from the legacy system modernization pipeline. Review and refine as needed with your team.*
