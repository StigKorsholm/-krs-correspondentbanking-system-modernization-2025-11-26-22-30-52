# Bounded Context Canvas: Correspondent Data Gateway

> **Generated from Legacy System Modernization Pipeline**  
> **BIAN Trinity Classification**: HYBRID  
> **Strategic Classification**: SUPPORTING

---

## 1. Name & Description

**Name**: Correspondent Data Gateway

**Description**: Provides a unified, secure, and versioned API gateway for all consumers of correspondent banking data. It handles legacy data format generation and acts as a facade over the domain-specific contexts.

**Trinity Role**: Technical Enabler. A facade that composes data from multiple domain contexts and exposes it in consumer-specific formats, including legacy files.

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

- **Treasury System**: Consumes a daily master data file.
- **SWIFT Gateway**: Queries for SSI and RMA data via APIs.
- **Developer**: Uses the gateway's APIs to build new applications.

---

## 6. Inbound Communication

**What comes INTO this context:**

- **Treasury System**
  - Message: `GET /data-feeds/corr-master`
  - Protocol: sync-rest-api
  - Reason: To request the daily master data file.

- **All Contexts**
  - Message: `Domain events (e.g., SSIUpdated)`
  - Protocol: async-event-kafka
  - Reason: To invalidate caches and trigger data feed regeneration.

---

## 7. Outbound Communication

**What goes OUT of this context:**

- **Settlement Instructions Management**
  - Message: `GetActiveSSIForPayment query`
  - Protocol: sync-rest-api
  - Reason: To fulfill a request from an external consumer.

- **Legacy Consumers**
  - Message: `CORR_MASTER file`
  - Protocol: batch-file-sftp
  - Reason: To provide data in the required legacy format.

---

## 8. Ubiquitous Language

**Domain-specific terminology:**

**api_contract**: The defined structure and behavior of an API endpoint.

**consumer**: Any system or user that requests data from the gateway.

**data_feed**: A recurring, often batch, export of data in a specific format.

**legacy_format**: A data structure required by an older, consuming system (e.g., CORR_MASTER file).

---

## 9. Responsibilities

**What this context does:**

- Expose a unified REST API for all correspondent banking data.
- Authenticate and authorize all incoming requests.
- Route requests to the appropriate downstream bounded context.
- Generate legacy file formats (e.g., CORR_MASTER) for backward compatibility.
- Orchestrate end-of-day batch processing and report generation.
- Provide comprehensive monitoring, logging, and tracing for all data access.

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

- CAP-005

---

## Key Non-Functional Requirements

- NFR-004
- NFR-007
- NFR-009
- NFR-011
- NFR-012
- NFR-013
- NFR-014

---

## Context Relationships

### Upstream Dependencies (This context depends on)

- **All Business Contexts** (upstream-downstream) - Business contexts (upstream) publish events about state changes, which the Gateway (downstream) consumes to update its views.

### Downstream Consumers (This context provides to)

None - This context has no downstream consumers

---

## Legacy System Integration

### Modernization Strategy

**Strategy**: strangler-fig

🌿 **Strangler Fig** - Gradually replace legacy functions over time




### Legacy Artifacts Covered

- Exports to SWIFT gateway and treasury system
- RPT_CORR_DAILY
- CORR_MASTER

### Integration Points

No integration points defined yet.

---

## Notes

This is a technical context that implements the facade and API Gateway patterns. It should be stateless to allow for horizontal scaling. It owns no data itself.

---

## Context Metadata

- **Context ID**: bc-005
- **Generated**: 2025-11-26T17:29:44.186Z
- **Source**: Legacy System Modernization Pipeline (BIAN v13 + DDD)

---

## Quick Reference Card

| Aspect | Value |
|--------|-------|
| **Name** | Correspondent Data Gateway |
| **Type** | supporting |
| **Trinity** | hybrid |
| **BIAN Layer** | Technical |
| **Evolution** | custom-built |
| **Aggregates** | 0 |
| **Capabilities** | 1 |
| **NFRs** | 7 |
| **Upstream Deps** | 1 |
| **Downstream Consumers** | 0 |

---

*This Canvas v5 document was auto-generated from the legacy system modernization pipeline. Review and refine as needed with your team.*
