# Bounded Context Canvas: Clearing System Directory

> **Generated from Legacy System Modernization Pipeline**  
> **BIAN Trinity Classification**: MEMORY  
> **Strategic Classification**: SUPPORTING

---

## 1. Name & Description

**Name**: Clearing System Directory

**Description**: A specialized reference data context that manages details of payment clearing systems (e.g., TARGET2, Fedwire) and their participants. It isolates this highly stable, technical reference data.

**Trinity Role**: System of Record. Provides a canonical source for clearing system and participant data, which changes infrequently.

---

## 2. Strategic Classification

**Type**: supporting


⚙️ **Supporting Domain** - Necessary but not differentiating


---

## 3. Business Model

**Revenue Impact**: none

**Cost Center**: technology-platform

---

## 4. Evolution Stage

**Stage**: product



📦 Product - Standardized, configurable, proven solutions


---

## 5. Domain Roles

**Who interacts with this bounded context:**

- **System Administrator**: Updates participant data based on circulars from clearing houses.
- **Settlement Instructions Management**: Queries for valid participant data when constructing SSIs.

---

## 6. Inbound Communication

**What comes INTO this context:**

- **Manual Upload**
  - Message: `UpdateParticipantDirectory batch file`
  - Protocol: batch-file-sftp
  - Reason: To apply periodic updates from clearing system operators.

---

## 7. Outbound Communication

**What goes OUT of this context:**

- **Settlement Instructions Management**
  - Message: `GetParticipantDetails query`
  - Protocol: sync-rest-api
  - Reason: To validate and enrich SSIs with correct clearing system data.

---

## 8. Ubiquitous Language

**Domain-specific terminology:**

**rtgs**: Real-Time Gross Settlement, a type of clearing system.

**target2**: The RTGS system for the Eurozone.

**participant_bic**: The BIC identifying a direct participant in a clearing system.

**addressee_bic**: The specific BIC to be used in the message header for a participant.

---

## 9. Responsibilities

**What this context does:**

- Maintain a directory of supported clearing systems.
- Manage the list of participants for each clearing system.
- Store system-specific identifiers and BICs for each participant.
- Provide validated participant data to other contexts.

---

## 10. Aggregates & Entities

**Core domain objects owned by this context:**

### ClearingSystemParticipant

**Value Objects**: BIC, ClearingSystemName, Currency



---

## BIAN v13 Alignment

### Owned BIAN Service Domains

No BIAN domains owned by this context.

### BIAN Layer

**Layer**: Reference Data

### Trinity Classification

**Classification**: memory

💾 **MEMORY** - System of Record, Reference Data




---

## Related Capabilities

- Implicit in CAP-003

---

## Key Non-Functional Requirements

- NFR-005
- NFR-006

---

## Context Relationships

### Upstream Dependencies (This context depends on)

None - This context has no upstream dependencies

### Downstream Consumers (This context provides to)

- **Settlement Instructions Management** (upstream-downstream) - The Directory (upstream) provides reference data about clearing systems used to validate and build SSIs (downstream).

---

## Legacy System Integration

### Modernization Strategy

**Strategy**: big-bang



💥 **Big Bang** - Replace entire context at once (high risk)


### Legacy Artifacts Covered

- KRSTB057
- KRSTB058

### Integration Points

No integration points defined yet.

---

## Notes

This is a pure 'MEMORY' context. The data is static and changes rarely. A simple CRUD service with heavy caching is likely sufficient. A 'big-bang' migration is feasible due to the static nature of the data.

---

## Context Metadata

- **Context ID**: bc-004
- **Generated**: 2025-11-26T17:29:44.185Z
- **Source**: Legacy System Modernization Pipeline (BIAN v13 + DDD)

---

## Quick Reference Card

| Aspect | Value |
|--------|-------|
| **Name** | Clearing System Directory |
| **Type** | supporting |
| **Trinity** | memory |
| **BIAN Layer** | Reference Data |
| **Evolution** | product |
| **Aggregates** | 1 |
| **Capabilities** | 1 |
| **NFRs** | 2 |
| **Upstream Deps** | 0 |
| **Downstream Consumers** | 1 |

---

*This Canvas v5 document was auto-generated from the legacy system modernization pipeline. Review and refine as needed with your team.*
