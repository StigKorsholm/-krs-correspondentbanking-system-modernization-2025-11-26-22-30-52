# Bounded Context Canvas: Legacy Integration (Anti-Corruption Layer)

> **Generated from Legacy System Modernization Pipeline**  
> **BIAN Trinity Classification**: HYBRID  
> **Strategic Classification**: SUPPORTING

---

## 1. Name & Description

**Name**: Legacy Integration (Anti-Corruption Layer)

**Description**: Isolates the modernized system from the legacy KRS system during the migration phase. It handles data translation, dual-write coordination, and provides a safety net for rollback.

**Trinity Role**: Migration Enabler. A temporary but critical context that translates between the new canonical domain models and the legacy data structures, enabling a phased migration.

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

**Stage**: custom-built


🔧 Custom-Built - Unique to organization, competitive differentiator



---

## 5. Domain Roles

**Who interacts with this bounded context:**

- **Modernized Contexts**: Send events/commands to the ACL for legacy synchronization.
- **Legacy KRS Database**: Receives data writes from the ACL.
- **Migration Team**: Monitors the health and consistency of the dual-write process.

---

## 6. Inbound Communication

**What comes INTO this context:**

- **All Modernized Contexts**
  - Message: `All CUD domain events (e.g., CorrespondentCreated, SSIUpdated)`
  - Protocol: async-event-kafka
  - Reason: To trigger a dual-write to the legacy KRS database.

---

## 7. Outbound Communication

**What goes OUT of this context:**

- **Legacy KRS Database**
  - Message: `SQL INSERT/UPDATE/DELETE statements`
  - Protocol: sync-db-connector
  - Reason: To persist state changes in the legacy system.

- **Monitoring Dashboard**
  - Message: `SynchronizationLag metric`
  - Protocol: metrics-prometheus
  - Reason: To provide operational visibility into the migration process.

---

## 8. Ubiquitous Language

**Domain-specific terminology:**

**dual_write**: The practice of writing data to both the new and legacy systems simultaneously to keep them in sync.

**data_translation**: Converting data from the new context's model to the legacy table structure, and vice-versa.

**synchronization_lag**: The time delay between a write to the new system and the corresponding write to the legacy system.

**strangler_fig**: A migration pattern where the new system gradually grows over and replaces the old system.

---

## 9. Responsibilities

**What this context does:**

- Translate domain events from new contexts into legacy database writes.
- Implement idempotent write operations to the legacy system.
- Monitor for and alert on data inconsistencies between new and legacy systems.
- Provide a mechanism to read data from the legacy system for functionalities not yet migrated.
- Facilitate the eventual decommissioning of legacy integration points.

---

## 10. Aggregates & Entities

**Core domain objects owned by this context:**

No aggregates defined yet.

---

## BIAN v13 Alignment

### Owned BIAN Service Domains

No BIAN domains owned by this context.

### BIAN Layer

**Layer**: Technical

### Trinity Classification

**Classification**: hybrid




⚙️ **HYBRID** - Mix of Memory/Heart/Brain characteristics

---

## Related Capabilities

- all capabilities during migration

---

## Key Non-Functional Requirements

- NFR-009
- NFR-010
- NFR-014

---

## Context Relationships

### Upstream Dependencies (This context depends on)

None - This context has no upstream dependencies

### Downstream Consumers (This context provides to)

None - This context has no downstream consumers

---

## Legacy System Integration

### Modernization Strategy

**Strategy**: strangler-fig

🌿 **Strangler Fig** - Gradually replace legacy functions over time




### Legacy Artifacts Covered

- Direct database access to all KRSTB* tables

### Integration Points

No integration points defined yet.

---

## Notes

This context is temporary and designed to be decommissioned once the migration is complete. Its sole purpose is to de-risk the migration process.

---

## Context Metadata

- **Context ID**: bc-006
- **Generated**: 2025-11-26T17:29:44.186Z
- **Source**: Legacy System Modernization Pipeline (BIAN v13 + DDD)

---

## Quick Reference Card

| Aspect | Value |
|--------|-------|
| **Name** | Legacy Integration (Anti-Corruption Layer) |
| **Type** | supporting |
| **Trinity** | hybrid |
| **BIAN Layer** | Technical |
| **Evolution** | custom-built |
| **Aggregates** | 0 |
| **Capabilities** | 1 |
| **NFRs** | 3 |
| **Upstream Deps** | 0 |
| **Downstream Consumers** | 0 |

---

*This Canvas v5 document was auto-generated from the legacy system modernization pipeline. Review and refine as needed with your team.*
