# API Catalog: KRS Correspondentbanking system

**Generated:** 2025-11-26  
**Total APIs:** 6  
**Total Endpoints:** 29

---

## Table of Contents

1. [Overview](#overview)
2. [API Summary](#api-summary)
3. [APIs by Microservice](#apis-by-microservice)
4. [All Endpoints](#all-endpoints)
5. [Schemas](#schemas)
6. [Security](#security)
7. [API Versioning](#api-versioning)
8. [Testing APIs](#testing-apis)

---

## Overview

This document catalogs all REST APIs across the modernized KRS Correspondentbanking system system. Each microservice exposes APIs following OpenAPI 3.1 specification with:

- **RESTful design principles** (resource-oriented URLs)
- **Standard HTTP methods** (GET, POST, PUT, PATCH, DELETE)
- **JSON request/response** (application/json)
- **OAuth 2.0 authentication** (JWT bearer tokens)
- **Comprehensive error responses** (standard error format)
- **API versioning** (URL-based: /api/v1/)

### API Design Standards

- **Base URL Pattern:** `https://api.example.com/v1`
- **Authentication:** OAuth 2.0 / JWT in `Authorization: Bearer {token}` header
- **Content-Type:** `application/json`
- **Rate Limiting:** 100 requests/second per client (configurable per service)
- **Pagination:** Query parameters `page`, `size`, `sort`
- **Error Format:** RFC 7807 Problem Details

---

## API Summary

### By HTTP Method

| Method | Count | Usage |
|--------|-------|-------|
| **GET** | 20 | Read operations |
| **POST** | 6 | Create operations |
| **PUT** | 2 | Full update operations |
| **PATCH** | 0 | Partial update operations |
| **DELETE** | 0 | Delete operations |

### By Service Type

| Type | Services | Avg Endpoints |
|------|----------|---------------|
| **other** | 3 | 9.7 |
| **query** | 2 | 14.5 |
| **command** | 2 | 14.5 |
| **gateway** | 1 | 29 |

### Security Coverage

- **APIs with Security:** 8/6
- **Coverage:** 100%
- **Security Schemes:** bearerAuth

---

## APIs by Microservice

### 1. Correspondent Master Data API

**Service:** correspondent-master-data-api  
**Version:** 1.0.0  
**Base URL:** https://api.example.bank/correspondent-master-data  
**Type:** api-combined  
**Bounded Context:** Correspondent Relationship Management

**Description:** Manages the master record for each Correspondent Bank entity, including lifecycle, validation, and audit trails. Acts as the authoritative source for 'who' a correspondent is.

**Endpoints:** 6  
**Schemas:** 4

#### Endpoints

- **POST** `/api/v1/correspondents` - Onboard a new correspondent bank
- **GET** `/api/v1/correspondents/{id}` - Get correspondent details by internal ID
- **PUT** `/api/v1/correspondents/{id}/status` - Update the status of a correspondent relationship
- **GET** `/actuator/health` - Application health check
- **GET** `/actuator/health/liveness` - Kubernetes liveness probe
- **GET** `/actuator/health/readiness` - Kubernetes readiness probe

#### Key Schemas

- **CorrespondentDTO** (object) - 7 required fields
- **CreateCorrespondentRequest** (object) - 3 required fields
- **UpdateCorrespondentStatusRequest** (object) - 1 required fields
- **ErrorResponse** (object) - 5 required fields

**OpenAPI Spec:** [`correspondent-master-data-api.openapi.json`](./api-specs/correspondent-master-data-api.openapi.json)

---

### 2. RMA Management API

**Service:** rma-management-api  
**Version:** 1.0.0  
**Base URL:** https://api.example.bank/rma-management  
**Type:** api-combined  
**Bounded Context:** Correspondent Relationship Management

**Description:** Manages SWIFT RMA (Relationship Management Application) authorizations, defining which message types can be exchanged with correspondents.

**Endpoints:** 5  
**Schemas:** 4

#### Endpoints

- **GET** `/api/v1/rma/authorizations` - Query RMA authorizations
- **POST** `/api/v1/rma/authorizations` - Manually create or update an RMA authorization
- **GET** `/actuator/health` - Application health check
- **GET** `/actuator/health/liveness` - Kubernetes liveness probe
- **GET** `/actuator/health/readiness` - Kubernetes readiness probe

#### Key Schemas

- **RmaAuthorizationDTO** (object) - 0 required fields
- **CreateRmaAuthorizationRequest** (object) - 4 required fields
- **RmaQueryResponseDTO** (object) - 0 required fields
- **ErrorResponse** (object) - 5 required fields

**OpenAPI Spec:** [`rma-management-api.openapi.json`](./api-specs/rma-management-api.openapi.json)

---

### 3. SSI Query API

**Service:** ssi-query-api  
**Version:** 1.0.0  
**Base URL:** https://api.example.bank/ssi-query  
**Type:** api-query  
**Bounded Context:** Settlement Instructions Management

**Description:** Read-optimized, low-latency API for querying Standard Settlement Instructions (SSIs). Designed for high-throughput reads from payment engines.

**Endpoints:** 5  
**Schemas:** 3

#### Endpoints

- **GET** `/api/v1/ssis/active` - Get the active SSI for a payment context
- **GET** `/api/v1/ssis/{id}` - Get a specific SSI by its unique ID
- **GET** `/actuator/health` - Application health check
- **GET** `/actuator/health/liveness` - Kubernetes liveness probe
- **GET** `/actuator/health/readiness` - Kubernetes readiness probe

#### Key Schemas

- **SsiDTO** (object) - 0 required fields
- **RoutingStepDTO** (object) - 0 required fields
- **ErrorResponse** (object) - 5 required fields

**OpenAPI Spec:** [`ssi-query-api.openapi.json`](./api-specs/ssi-query-api.openapi.json)

---

### 4. SSI Command API

**Service:** ssi-command-api  
**Version:** 1.0.0  
**Base URL:** https://api.example.bank/ssi-command  
**Type:** api-command  
**Bounded Context:** Settlement Instructions Management

**Description:** Handles the creation, update, and validation of Standard Settlement Instructions (SSIs). Ensures data integrity and business rule enforcement.

**Endpoints:** 5  
**Schemas:** 5

#### Endpoints

- **POST** `/api/v1/ssis` - Create a new Standard Settlement Instruction
- **PUT** `/api/v1/ssis/{id}` - Update an existing SSI
- **GET** `/actuator/health` - Application health check
- **GET** `/actuator/health/liveness` - Kubernetes liveness probe
- **GET** `/actuator/health/readiness` - Kubernetes readiness probe

#### Key Schemas

- **CreateSsiRequest** (object) - 4 required fields
- **UpdateSsiRequest** (object) - 0 required fields
- **RoutingStepDTO** (object) - 0 required fields
- **AsyncJobStatus** (object) - 0 required fields
- **ErrorResponse** (object) - 5 required fields

**OpenAPI Spec:** [`ssi-command-api.openapi.json`](./api-specs/ssi-command-api.openapi.json)

---

### 5. Nostro Account API

**Service:** nostro-account-api  
**Version:** 1.0.0  
**Base URL:** https://api.example.bank/nostro-accounts  
**Type:** api-combined  
**Bounded Context:** Nostro Account Management

**Description:** Manages the bank's Nostro accounts held at correspondent banks. Handles master data and operational rules.

**Endpoints:** 5  
**Schemas:** 3

#### Endpoints

- **POST** `/api/v1/nostro-accounts` - Open a new Nostro account
- **GET** `/api/v1/nostro-accounts/{id}` - Get details for a specific Nostro account
- **GET** `/actuator/health` - Application health check
- **GET** `/actuator/health/liveness` - Kubernetes liveness probe
- **GET** `/actuator/health/readiness` - Kubernetes readiness probe

#### Key Schemas

- **NostroAccountDTO** (object) - 0 required fields
- **CreateNostroAccountRequest** (object) - 4 required fields
- **ErrorResponse** (object) - 5 required fields

**OpenAPI Spec:** [`nostro-account-api.openapi.json`](./api-specs/nostro-account-api.openapi.json)

---

### 6. Clearing Directory Query API

**Service:** clearing-directory-query-api  
**Version:** 1.0.0  
**Base URL:** https://api.example.bank/clearing-directory-query  
**Type:** api-query  
**Bounded Context:** Clearing System Directory

**Description:** Highly available, read-optimized API for static reference data about clearing systems (e.g., TARGET2) and their participants.

**Endpoints:** 5  
**Schemas:** 3

#### Endpoints

- **GET** `/api/v1/clearing-systems/{systemName}/participants` - List all participants for a clearing system
- **GET** `/api/v1/clearing-systems/{systemName}/participants/{participantBic}` - Get details for a specific participant
- **GET** `/actuator/health` - Application health check
- **GET** `/actuator/health/liveness` - Kubernetes liveness probe
- **GET** `/actuator/health/readiness` - Kubernetes readiness probe

#### Key Schemas

- **ParticipantDTO** (object) - 0 required fields
- **PagedParticipantsResponse** (object) - 0 required fields
- **ErrorResponse** (object) - 5 required fields

**OpenAPI Spec:** [`clearing-directory-query-api.openapi.json`](./api-specs/clearing-directory-query-api.openapi.json)

---

### 7. Clearing Directory Command API

**Service:** clearing-directory-command-api  
**Version:** 1.0.0  
**Base URL:** https://api.example.bank/clearing-directory-command  
**Type:** api-command  
**Bounded Context:** Clearing System Directory

**Description:** Handles write operations for the clearing system directory. Primarily used by administrators and sync workers.

**Endpoints:** 4  
**Schemas:** 4

#### Endpoints

- **POST** `/api/v1/clearing-systems/participants/bulk-update` - Replace the participant list for a clearing system
- **GET** `/actuator/health` - Application health check
- **GET** `/actuator/health/liveness` - Kubernetes liveness probe
- **GET** `/actuator/health/readiness` - Kubernetes readiness probe

#### Key Schemas

- **BulkUpdateRequest** (object) - 2 required fields
- **ParticipantDataDTO** (object) - 2 required fields
- **BulkUpdateStatusDTO** (object) - 0 required fields
- **ErrorResponse** (object) - 5 required fields

**OpenAPI Spec:** [`clearing-directory-command-api.openapi.json`](./api-specs/clearing-directory-command-api.openapi.json)

---

### 8. Correspondent Data Gateway

**Service:** correspondent-data-gateway  
**Version:** 1.0.0  
**Base URL:** https://api.example.bank/gateway  
**Type:** gateway  
**Bounded Context:** Correspondent Data Gateway

**Description:** A unified, secure API gateway providing a facade over the domain microservices. Handles authentication, routing, and rate limiting for external consumers.

**Endpoints:** 3  
**Schemas:** 5

#### Endpoints

- **GET** `/api/gateway/v1/correspondents/{bic}` - Unified search for correspondent data
- **GET** `/api/gateway/v1/ssis/active` - Proxy to the SSI Query API
- **GET** `/health` - Application health check

#### Key Schemas

- **UnifiedCorrespondentDTO** (object) - 0 required fields
- **SsiDTO** (object) - 0 required fields
- **RmaAuthorizationDTO** (object) - 0 required fields
- **NostroAccountDTO** (object) - 0 required fields
- **ErrorResponse** (object) - 5 required fields

**OpenAPI Spec:** [`correspondent-data-gateway.openapi.json`](./api-specs/correspondent-data-gateway.openapi.json)

---


## All Endpoints

Complete endpoint reference across all services:

### POST /api/v1/correspondents

**Service:** correspondent-master-data-api  
**Operation ID:** onboardCorrespondent  
**Summary:** Onboard a new correspondent bank  
**Tags:** Correspondents  
**Security Required:** No  


---

### GET /api/v1/correspondents/{id}

**Service:** correspondent-master-data-api  
**Operation ID:** getCorrespondentById  
**Summary:** Get correspondent details by internal ID  
**Tags:** Correspondents  
**Security Required:** No  


---

### PUT /api/v1/correspondents/{id}/status

**Service:** correspondent-master-data-api  
**Operation ID:** updateCorrespondentStatus  
**Summary:** Update the status of a correspondent relationship  
**Tags:** Correspondents  
**Security Required:** No  


---

### GET /actuator/health

**Service:** correspondent-master-data-api  
**Operation ID:** healthCheck  
**Summary:** Application health check  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/liveness

**Service:** correspondent-master-data-api  
**Operation ID:** livenessProbe  
**Summary:** Kubernetes liveness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/readiness

**Service:** correspondent-master-data-api  
**Operation ID:** readinessProbe  
**Summary:** Kubernetes readiness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /api/v1/rma/authorizations

**Service:** rma-management-api  
**Operation ID:** queryRmaAuthorizations  
**Summary:** Query RMA authorizations  
**Tags:** RMA  
**Security Required:** No  


---

### POST /api/v1/rma/authorizations

**Service:** rma-management-api  
**Operation ID:** createOrUpdateRmaAuthorization  
**Summary:** Manually create or update an RMA authorization  
**Tags:** RMA  
**Security Required:** No  


---

### GET /actuator/health

**Service:** rma-management-api  
**Operation ID:** healthCheck  
**Summary:** Application health check  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/liveness

**Service:** rma-management-api  
**Operation ID:** livenessProbe  
**Summary:** Kubernetes liveness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/readiness

**Service:** rma-management-api  
**Operation ID:** readinessProbe  
**Summary:** Kubernetes readiness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /api/v1/ssis/active

**Service:** ssi-query-api  
**Operation ID:** getActiveSsi  
**Summary:** Get the active SSI for a payment context  
**Tags:** SSIs  
**Security Required:** No  


---

### GET /api/v1/ssis/{id}

**Service:** ssi-query-api  
**Operation ID:** getSsiById  
**Summary:** Get a specific SSI by its unique ID  
**Tags:** SSIs  
**Security Required:** No  


---

### GET /actuator/health

**Service:** ssi-query-api  
**Operation ID:** healthCheck  
**Summary:** Application health check  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/liveness

**Service:** ssi-query-api  
**Operation ID:** livenessProbe  
**Summary:** Kubernetes liveness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/readiness

**Service:** ssi-query-api  
**Operation ID:** readinessProbe  
**Summary:** Kubernetes readiness probe  
**Tags:** Operations  
**Security Required:** No  


---

### POST /api/v1/ssis

**Service:** ssi-command-api  
**Operation ID:** createSsi  
**Summary:** Create a new Standard Settlement Instruction  
**Tags:** SSIs  
**Security Required:** No  


---

### PUT /api/v1/ssis/{id}

**Service:** ssi-command-api  
**Operation ID:** updateSsi  
**Summary:** Update an existing SSI  
**Tags:** SSIs  
**Security Required:** No  


---

### GET /actuator/health

**Service:** ssi-command-api  
**Operation ID:** healthCheck  
**Summary:** Application health check  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/liveness

**Service:** ssi-command-api  
**Operation ID:** livenessProbe  
**Summary:** Kubernetes liveness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/readiness

**Service:** ssi-command-api  
**Operation ID:** readinessProbe  
**Summary:** Kubernetes readiness probe  
**Tags:** Operations  
**Security Required:** No  


---

### POST /api/v1/nostro-accounts

**Service:** nostro-account-api  
**Operation ID:** openNostroAccount  
**Summary:** Open a new Nostro account  
**Tags:** Nostro Accounts  
**Security Required:** No  


---

### GET /api/v1/nostro-accounts/{id}

**Service:** nostro-account-api  
**Operation ID:** getNostroAccountById  
**Summary:** Get details for a specific Nostro account  
**Tags:** Nostro Accounts  
**Security Required:** No  


---

### GET /actuator/health

**Service:** nostro-account-api  
**Operation ID:** healthCheck  
**Summary:** Application health check  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/liveness

**Service:** nostro-account-api  
**Operation ID:** livenessProbe  
**Summary:** Kubernetes liveness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/readiness

**Service:** nostro-account-api  
**Operation ID:** readinessProbe  
**Summary:** Kubernetes readiness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /api/v1/clearing-systems/{systemName}/participants

**Service:** clearing-directory-query-api  
**Operation ID:** listParticipants  
**Summary:** List all participants for a clearing system  
**Tags:** Clearing Directory  
**Security Required:** No  


---

### GET /api/v1/clearing-systems/{systemName}/participants/{participantBic}

**Service:** clearing-directory-query-api  
**Operation ID:** getParticipantDetails  
**Summary:** Get details for a specific participant  
**Tags:** Clearing Directory  
**Security Required:** No  


---

### GET /actuator/health

**Service:** clearing-directory-query-api  
**Operation ID:** healthCheck  
**Summary:** Application health check  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/liveness

**Service:** clearing-directory-query-api  
**Operation ID:** livenessProbe  
**Summary:** Kubernetes liveness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/readiness

**Service:** clearing-directory-query-api  
**Operation ID:** readinessProbe  
**Summary:** Kubernetes readiness probe  
**Tags:** Operations  
**Security Required:** No  


---

### POST /api/v1/clearing-systems/participants/bulk-update

**Service:** clearing-directory-command-api  
**Operation ID:** bulkUpdateParticipants  
**Summary:** Replace the participant list for a clearing system  
**Tags:** Clearing Directory  
**Security Required:** No  


---

### GET /actuator/health

**Service:** clearing-directory-command-api  
**Operation ID:** healthCheck  
**Summary:** Application health check  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/liveness

**Service:** clearing-directory-command-api  
**Operation ID:** livenessProbe  
**Summary:** Kubernetes liveness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /actuator/health/readiness

**Service:** clearing-directory-command-api  
**Operation ID:** readinessProbe  
**Summary:** Kubernetes readiness probe  
**Tags:** Operations  
**Security Required:** No  


---

### GET /api/gateway/v1/correspondents/{bic}

**Service:** correspondent-data-gateway  
**Operation ID:** getUnifiedCorrespondentData  
**Summary:** Unified search for correspondent data  
**Tags:** Gateway  
**Security Required:** No  


---

### GET /api/gateway/v1/ssis/active

**Service:** correspondent-data-gateway  
**Operation ID:** getActiveSsiViaGateway  
**Summary:** Proxy to the SSI Query API  
**Tags:** Gateway  
**Security Required:** No  


---

### GET /health

**Service:** correspondent-data-gateway  
**Operation ID:** healthCheck  
**Summary:** Application health check  
**Tags:** Operations  
**Security Required:** No  


---


## Schemas

Data transfer objects (DTOs) used across all APIs:

### CorrespondentDTO

**Type:** object  
**Required Fields:** id, bic, legalName, countryCode, status, createdAt, updatedAt  
**Properties:** 8  
**Used by:** correspondent-master-data-api

**Description:** Represents the master data for a correspondent bank.

---

### CreateCorrespondentRequest

**Type:** object  
**Required Fields:** bic, legalName, countryCode  
**Properties:** 4  
**Used by:** correspondent-master-data-api

**Description:** Payload for creating a new correspondent bank.

---

### UpdateCorrespondentStatusRequest

**Type:** object  
**Required Fields:** status  
**Properties:** 2  
**Used by:** correspondent-master-data-api

**Description:** Payload for updating the status of a correspondent.

---

### ErrorResponse

**Type:** object  
**Required Fields:** timestamp, status, error, message, path  
**Properties:** 6  
**Used by:** correspondent-master-data-api, rma-management-api, ssi-query-api, ssi-command-api, nostro-account-api, clearing-directory-query-api, clearing-directory-command-api, correspondent-data-gateway



---

### RmaAuthorizationDTO

**Type:** object  
**Required Fields:** None  
**Properties:** 7  
**Used by:** rma-management-api, correspondent-data-gateway

**Description:** Represents a single SWIFT RMA authorization.

---

### CreateRmaAuthorizationRequest

**Type:** object  
**Required Fields:** correspondentBic, messageType, direction, status  
**Properties:** 6  
**Used by:** rma-management-api



---

### RmaQueryResponseDTO

**Type:** object  
**Required Fields:** None  
**Properties:** 1  
**Used by:** rma-management-api



---

### SsiDTO

**Type:** object  
**Required Fields:** None  
**Properties:** 8  
**Used by:** ssi-query-api, correspondent-data-gateway

**Description:** Represents a Standard Settlement Instruction.

---

### RoutingStepDTO

**Type:** object  
**Required Fields:** None  
**Properties:** 5  
**Used by:** ssi-query-api, ssi-command-api

**Description:** A single step in a payment routing chain.

---

### CreateSsiRequest

**Type:** object  
**Required Fields:** counterpartyId, currency, product, routingPath  
**Properties:** 4  
**Used by:** ssi-command-api



---

### UpdateSsiRequest

**Type:** object  
**Required Fields:** None  
**Properties:** 2  
**Used by:** ssi-command-api



---

### AsyncJobStatus

**Type:** object  
**Required Fields:** None  
**Properties:** 3  
**Used by:** ssi-command-api



---

### NostroAccountDTO

**Type:** object  
**Required Fields:** None  
**Properties:** 6  
**Used by:** nostro-account-api, correspondent-data-gateway



---

### CreateNostroAccountRequest

**Type:** object  
**Required Fields:** correspondentId, accountNumber, currency, accountType  
**Properties:** 4  
**Used by:** nostro-account-api



---

### ParticipantDTO

**Type:** object  
**Required Fields:** None  
**Properties:** 4  
**Used by:** clearing-directory-query-api



---

### PagedParticipantsResponse

**Type:** object  
**Required Fields:** None  
**Properties:** 2  
**Used by:** clearing-directory-query-api



---

### BulkUpdateRequest

**Type:** object  
**Required Fields:** clearingSystemName, participants  
**Properties:** 2  
**Used by:** clearing-directory-command-api



---

### ParticipantDataDTO

**Type:** object  
**Required Fields:** participantBic, participantName  
**Properties:** 2  
**Used by:** clearing-directory-command-api



---

### BulkUpdateStatusDTO

**Type:** object  
**Required Fields:** None  
**Properties:** 4  
**Used by:** clearing-directory-command-api



---

### UnifiedCorrespondentDTO

**Type:** object  
**Required Fields:** None  
**Properties:** 7  
**Used by:** correspondent-data-gateway



---


## Security

### Authentication

All APIs use **OAuth 2.0** with **JWT bearer tokens**.

**Header Format:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Token Endpoint:** `POST /auth/token`

**Request:**
```json
{
  "grant_type": "client_credentials",
  "client_id": "your-client-id",
  "client_secret": "your-client-secret",
  "scope": "read:correspondents write:correspondents"
}
```

**Response:**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "read:correspondents write:correspondents"
}
```

### Authorization Scopes

Common scopes across APIs:

- `read:correspondents` - Read correspondent data
- `write:correspondents` - Create/update correspondent data
- `read:relationships` - Read relationship data
- `write:relationships` - Create/update relationship data
- `read:transactions` - Read transaction data
- `write:transactions` - Create/update transaction data
- `admin` - Full administrative access

### Rate Limiting

**Default Limits:**
- 100 requests/second per client
- 10,000 requests/hour per client
- Burst: 200 requests

**Headers:**
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1701086400
```

**429 Response (Too Many Requests):**
```json
{
  "timestamp": "2024-11-27T10:30:00Z",
  "status": 429,
  "error": "Too Many Requests",
  "message": "Rate limit exceeded. Retry after 60 seconds.",
  "path": "/api/v1/correspondents"
}
```

---

## API Versioning

**Strategy:** URL-based versioning

**Current Version:** v1  
**Base URL:** `/api/v1/...`

**Versions Found:** v1  
**Consistent Versioning:** Yes ✅

### Version Lifecycle

- **v1:** Current production version
- **v2:** (Future) Breaking changes will be introduced in v2
- **Deprecation:** v1 will be deprecated 6 months after v2 release
- **Sunset:** v1 will be sunset 12 months after v2 release

---

## Testing APIs

### Using cURL

**Example: Get Correspondent by ID**
```bash
curl -X GET "https://api.example.com/v1/correspondents/550e8400-e29b-41d4-a716-446655440000" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json"
```

**Example: Search Correspondents**
```bash
curl -X GET "https://api.example.com/v1/correspondents?country=DE&page=0&size=20" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json"
```

**Example: Create Correspondent**
```bash
curl -X POST "https://api.example.com/v1/correspondents" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "bic": "DEUTDEFF",
    "lei": "529900HNOAA1KXQJUQ27",
    "name": "Deutsche Bank AG",
    "country": "DE"
  }'
```

### Using Postman

1. **Import OpenAPI Specs:**
   - Download spec files from `api-specs/` folder
   - In Postman: Import → Select spec file
   - Collection auto-generated with all endpoints

2. **Set Environment Variables:**
   - `base_url`: https://api.example.com/v1
   - `access_token`: (Your JWT token)

3. **Authorization:**
   - Type: Bearer Token
   - Token: `{{access_token}}`

### Using Swagger UI

Each microservice exposes Swagger UI at:
```
https://api.example.com/v1/swagger-ui/
```

**Features:**
- Interactive API documentation
- Try endpoints directly in browser
- View request/response schemas
- Test authentication

### Health Checks

All services expose health check endpoints (no auth required):

```
GET /actuator/health           # Overall health
GET /actuator/health/liveness  # Kubernetes liveness probe
GET /actuator/health/readiness # Kubernetes readiness probe
```

**Response:**
```json
{
  "status": "UP",
  "components": {
    "db": { "status": "UP" },
    "kafka": { "status": "UP" },
    "redis": { "status": "UP" }
  }
}
```

---

## Error Handling

### Standard Error Response

All APIs return errors in RFC 7807 Problem Details format:

```json
{
  "timestamp": "2024-11-27T10:30:00Z",
  "status": 400,
  "error": "Bad Request",
  "message": "Validation failed",
  "path": "/api/v1/correspondents",
  "validation_errors": [
    {
      "field": "bic",
      "message": "BIC code format is invalid"
    }
  ]
}
```

### HTTP Status Codes

| Code | Meaning | When Used |
|------|---------|-----------|
| **200** | OK | Successful GET/PUT/PATCH |
| **201** | Created | Successful POST (resource created) |
| **204** | No Content | Successful DELETE |
| **400** | Bad Request | Validation error |
| **401** | Unauthorized | Missing/invalid auth token |
| **403** | Forbidden | Insufficient permissions |
| **404** | Not Found | Resource doesn't exist |
| **409** | Conflict | Business rule violation |
| **429** | Too Many Requests | Rate limit exceeded |
| **500** | Internal Server Error | Server error |
| **503** | Service Unavailable | Service down/maintenance |

---

## API Statistics

- **Total Microservices with APIs:** 6
- **Total Endpoints:** 29
- **Total Schemas:** 31
- **Average Endpoints per Service:** 3.6
- **Average Schemas per Service:** 3.9
- **Most Complex Service:** correspondent-master-data-api (6 endpoints)

---

## Next Steps

1. **Review API Specs:** See `api-specs/` folder for complete OpenAPI specifications
2. **Set Up Authentication:** Configure OAuth 2.0 tokens
3. **Import to Postman:** Use OpenAPI specs to generate collections
4. **Test Endpoints:** Use Swagger UI or cURL examples
5. **Integrate:** See `DEVELOPER_GUIDE.md` for integration instructions

---

## Support

For API questions or issues:
- **Documentation:** See individual OpenAPI specs in `api-specs/` folder
- **Swagger UI:** https://api.example.com/v1/swagger-ui/
- **Contact:** architecture@yourbank.com

---

*This API catalog was auto-generated from the Legacy System Modernization Pipeline.*
