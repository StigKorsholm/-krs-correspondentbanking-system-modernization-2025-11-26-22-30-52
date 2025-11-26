# Database Schema Catalog: KRS Correspondentbanking system

**Generated:** 2025-11-26  
**Total Databases:** 7  
**Total Tables:** 15  
**Total Indexes:** 17

---

## Table of Contents

1. [Overview](#overview)
2. [Database Summary](#database-summary)
3. [Schemas by Microservice](#schemas-by-microservice)
4. [All Tables](#all-tables)
5. [Indexes](#indexes)
6. [Data Types](#data-types)
7. [Storage Estimates](#storage-estimates)
8. [Migration Scripts](#migration-scripts)
9. [Best Practices](#best-practices)

---

## Overview

This document catalogs all database schemas across the modernized KRS Correspondentbanking system system. Each microservice has its own PostgreSQL database with:

- **PostgreSQL 15+** as the target database
- **UUID primary keys** for distributed system compatibility
- **Audit columns** (created_at, updated_at, created_by) on all tables
- **Optimistic locking** (version column) for concurrency control
- **Indexes optimized** for API query patterns
- **Constraints** for data integrity at database level
- **Flyway migrations** for version control

### Database Design Principles

- **Database per Service:** Each microservice owns its database (no shared databases)
- **ACID Transactions:** Full transactional support for data integrity
- **Denormalization:** Where needed for query performance (read models)
- **Event Sourcing:** Event store tables for services publishing domain events
- **Audit Trail:** All changes tracked with timestamps and user IDs
- **Type Safety:** Proper data types (DECIMAL for money, UUID for IDs, JSONB for flexible data)

---

## Database Summary

### By Microservice Type

- **correspondent_master_db**: 2 tables, 4 indexes
- **rma_management_db**: 3 tables, 1 indexes
- **ssi_read_db**: 1 tables, 1 indexes
- **ssi_write_db**: 3 tables, 1 indexes
- **nostro_account_db**: 3 tables, 1 indexes
- **N/A**: 0 tables, 0 indexes
- **clearing_directory_read_db**: 1 tables, 1 indexes
- **clearing_directory_write_db**: 2 tables, 0 indexes
- **N/A (Oracle)**: 0 tables, 0 indexes
- **N/A (AWS S3)**: 0 tables, 0 indexes

### Overall Statistics

| Metric | Count |
|--------|-------|
| **Total Databases** | 7 |
| **Total Tables** | 15 |
| **Total Indexes** | 17 |
| **Total Constraints** | 35 |
| **Total Migrations** | 12 |

### Averages

- **Tables per Service:** 1.2
- **Indexes per Table:** 1.1
- **Services with Event Store:** 5
- **Services with Audit Triggers:** 5

---

## Schemas by Microservice

### 1. correspondent_master_db

**Microservice:** correspondent-master-data-api  
**Schema:** public  
**Bounded Context:** Correspondent Relationship Management  
**BIAN Trinity:** N/A

**Tables:** 2  
**Views:** 0  
**Triggers:** 1  
**Migrations:** 3

#### Tables

- **correspondents** (CorrespondentBank) - 13 columns, 2 indexes
- **domain_events** (N/A (Cross-cutting concern)) - 9 columns, 2 indexes

#### Storage Estimate


- **Expected Rows:** 20,000
- **Initial Size:** 10 MB
- **Annual Growth:** 1 MB/year


#### Migration Files

- **V1__create_audit_function.sql**: Create a reusable function to update the updated_at timestamp.
- **V2__create_correspondents_table.sql**: Create the main table for correspondent master data.
- **V3__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

---

### 2. rma_management_db

**Microservice:** rma-management-api  
**Schema:** public  
**Bounded Context:** Correspondent Relationship Management  
**BIAN Trinity:** N/A

**Tables:** 3  
**Views:** 0  
**Triggers:** 2  
**Migrations:** 2

#### Tables

- **rma_agreements** (RelationshipManagementAgreement) - 10 columns, 0 indexes
- **authorized_messages** (RelationshipManagementAgreement) - 9 columns, 1 indexes
- **domain_events** (N/A (Cross-cutting concern)) - 0 columns, 0 indexes

#### Storage Estimate


- **Expected Rows:** 500,000
- **Initial Size:** 128 MB
- **Annual Growth:** 20 MB/year


#### Migration Files

- **V1__create_rma_tables.sql**: Create tables for RMA agreements and authorized messages.
- **V2__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

---

### 3. ssi_read_db

**Microservice:** ssi-query-api  
**Schema:** public  
**Bounded Context:** Settlement Instructions Management  
**BIAN Trinity:** N/A

**Tables:** 1  
**Views:** 0  
**Triggers:** 0  
**Migrations:** 1

#### Tables

- **ssi_read_model** (StandardSettlementInstruction) - 10 columns, 1 indexes

#### Storage Estimate


- **Expected Rows:** 2,000,000
- **Initial Size:** 4096 MB
- **Annual Growth:** 500 MB/year


#### Migration Files

- **V1__create_ssi_read_model_table.sql**: Create the denormalized read model for SSIs.

---

### 4. ssi_write_db

**Microservice:** ssi-command-api  
**Schema:** public  
**Bounded Context:** Settlement Instructions Management  
**BIAN Trinity:** N/A

**Tables:** 3  
**Views:** 0  
**Triggers:** 1  
**Migrations:** 2

#### Tables

- **standard_settlement_instructions** (StandardSettlementInstruction) - 13 columns, 0 indexes
- **ssi_routing_intermediaries** (StandardSettlementInstruction) - 7 columns, 1 indexes
- **domain_events** (N/A (Cross-cutting concern)) - 0 columns, 0 indexes

#### Storage Estimate


- **Expected Rows:** 2,000,000
- **Initial Size:** 2048 MB
- **Annual Growth:** 250 MB/year


#### Migration Files

- **V1__create_ssi_tables.sql**: Create normalized tables for the SSI aggregate.
- **V2__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

---

### 5. nostro_account_db

**Microservice:** nostro-account-api  
**Schema:** public  
**Bounded Context:** Nostro Account Management  
**BIAN Trinity:** N/A

**Tables:** 3  
**Views:** 0  
**Triggers:** 2  
**Migrations:** 2

#### Tables

- **nostro_accounts** (NostroAccount) - 11 columns, 1 indexes
- **statement_rules** (NostroAccount) - 9 columns, 0 indexes
- **domain_events** (N/A (Cross-cutting concern)) - 0 columns, 0 indexes

#### Storage Estimate


- **Expected Rows:** 5,000
- **Initial Size:** 2 MB
- **Annual Growth:** 0.5 MB/year


#### Migration Files

- **V1__create_nostro_tables.sql**: Create tables for Nostro accounts and their statement rules.
- **V2__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

---

### 6. N/A

**Microservice:** nostro-statement-worker  
**Schema:** N/A  
**Bounded Context:** Nostro Account Management  
**BIAN Trinity:** N/A

**Tables:** 0  
**Views:** 0  
**Triggers:** 0  
**Migrations:** 0

#### Tables



#### Storage Estimate


- **Expected Rows:** 0
- **Initial Size:** 0 MB
- **Annual Growth:** 0 MB/year


#### Migration Files

No migrations defined

---

### 7. clearing_directory_read_db

**Microservice:** clearing-directory-query-api  
**Schema:** public  
**Bounded Context:** Clearing System Directory  
**BIAN Trinity:** N/A

**Tables:** 1  
**Views:** 0  
**Triggers:** 0  
**Migrations:** 1

#### Tables

- **clearing_participants_read_model** (ClearingSystemParticipant) - 6 columns, 1 indexes

#### Storage Estimate


- **Expected Rows:** 100,000
- **Initial Size:** 50 MB
- **Annual Growth:** 5 MB/year


#### Migration Files

- **V1__create_clearing_participants_read_model.sql**: Create the denormalized read model for clearing participants.

---

### 8. clearing_directory_write_db

**Microservice:** clearing-directory-command-api  
**Schema:** public  
**Bounded Context:** Clearing System Directory  
**BIAN Trinity:** N/A

**Tables:** 2  
**Views:** 0  
**Triggers:** 1  
**Migrations:** 2

#### Tables

- **clearing_participants** (ClearingSystemParticipant) - 8 columns, 0 indexes
- **domain_events** (N/A (Cross-cutting concern)) - 0 columns, 0 indexes

#### Storage Estimate


- **Expected Rows:** 100,000
- **Initial Size:** 50 MB
- **Annual Growth:** 5 MB/year


#### Migration Files

- **V1__create_clearing_participants_table.sql**: Create the transactional table for clearing participants.
- **V2__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

---

### 9. N/A

**Microservice:** clearing-directory-sync-worker  
**Schema:** N/A  
**Bounded Context:** Clearing System Directory  
**BIAN Trinity:** N/A

**Tables:** 0  
**Views:** 0  
**Triggers:** 0  
**Migrations:** 0

#### Tables



#### Storage Estimate


- **Expected Rows:** 0
- **Initial Size:** 0 MB
- **Annual Growth:** 0 MB/year


#### Migration Files

No migrations defined

---

### 10. N/A

**Microservice:** correspondent-data-gateway  
**Schema:** N/A  
**Bounded Context:** Correspondent Data Gateway  
**BIAN Trinity:** N/A

**Tables:** 0  
**Views:** 0  
**Triggers:** 0  
**Migrations:** 0

#### Tables



#### Storage Estimate


- **Expected Rows:** 0
- **Initial Size:** 0 MB
- **Annual Growth:** 0 MB/year


#### Migration Files

No migrations defined

---

### 11. N/A

**Microservice:** legacy-feed-generator-worker  
**Schema:** N/A  
**Bounded Context:** Correspondent Data Gateway  
**BIAN Trinity:** N/A

**Tables:** 0  
**Views:** 0  
**Triggers:** 0  
**Migrations:** 0

#### Tables



#### Storage Estimate


- **Expected Rows:** 0
- **Initial Size:** 0 MB
- **Annual Growth:** 0 MB/year


#### Migration Files

No migrations defined

---

### 12. N/A (Oracle)

**Microservice:** legacy-krs-acl-worker  
**Schema:** N/A  
**Bounded Context:** Legacy Integration (Anti-Corruption Layer)  
**BIAN Trinity:** N/A

**Tables:** 0  
**Views:** 0  
**Triggers:** 0  
**Migrations:** 0

#### Tables



#### Storage Estimate


- **Expected Rows:** 0
- **Initial Size:** 0 MB
- **Annual Growth:** 0 MB/year


#### Migration Files

No migrations defined

---

### 13. N/A (AWS S3)

**Microservice:** audit-log-worker  
**Schema:** N/A  
**Bounded Context:** Cross-Cutting Concern  
**BIAN Trinity:** N/A

**Tables:** 0  
**Views:** 0  
**Triggers:** 0  
**Migrations:** 0

#### Tables



#### Storage Estimate


- **Expected Rows:** 0
- **Initial Size:** 0 MB
- **Annual Growth:** 0 MB/year


#### Migration Files

No migrations defined

---


## All Tables

Complete table reference across all databases:

### correspondents

**Database:** correspondent_master_db  
**Microservice:** correspondent-master-data-api  
**Aggregate:** CorrespondentBank  
**Columns:** 13  
**Indexes:** 2  
**Constraints:** 6  
**Audit Columns:** Yes ✅  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| id | UUID | No | PK | Internal unique identifier for the correspondent. |
| bic | VARCHAR(11) | No | UQ | Bank Identifier Code (ISO 9362). Natural business key. |
| lei | VARCHAR(20) | Yes | UQ | Legal Entity Identifier (ISO 17442). |
| legal_name | VARCHAR(255) | No | - | Official legal name of the entity. |
| short_name | VARCHAR(100) | Yes | - | Commonly used short name or trading name. |
| country_code | CHAR(2) | No | - | ISO 3166-1 alpha-2 country code of the correspondent's headquarters. |
| status | VARCHAR(20) | No | - | Lifecycle status of the correspondent relationship (e.g., PENDING, ACTIVE, SUSPENDED). |
| address | JSONB | Yes | - | Structured address information (street, city, postal code). |
| created_at | TIMESTAMPTZ | No | - | Timestamp of record creation. |
| updated_at | TIMESTAMPTZ | No | - | Timestamp of last record update. |

*... and 3 more columns*

#### Indexes

- **idx_correspondents_status** on (status) - To efficiently query correspondents by their lifecycle status.
- **idx_correspondents_country_code** on (country_code) - To efficiently query correspondents by country.

#### Constraints

- **pk_correspondents** (PRIMARY KEY): id
- **uq_correspondents_bic** (UNIQUE): bic
- **uq_correspondents_lei** (UNIQUE): lei
- **chk_correspondents_bic_format** (CHECK): bic ~ '^[A-Z]{4}[A-Z]{2}[A-Z0-9]{2}([A-Z0-9]{3})?$'
- **chk_correspondents_lei_format** (CHECK): lei IS NULL OR lei ~ '^[A-Z0-9]{20}$'
- **chk_correspondents_status** (CHECK): status IN ('PENDING', 'ACTIVE', 'SUSPENDED', 'TERMINATED')


**DDL:** See migration files in `database-schemas/correspondent_master_db/`

---

### domain_events

**Database:** correspondent_master_db  
**Microservice:** correspondent-master-data-api  
**Aggregate:** N/A (Cross-cutting concern)  
**Columns:** 9  
**Indexes:** 2  
**Constraints:** 2  
**Audit Columns:** No ⚠️  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| id | UUID | No | PK | Unique identifier for the event record. |
| aggregate_type | VARCHAR(100) | No | - | The type of the aggregate that produced the event (e.g., 'CorrespondentBank'). |
| aggregate_id | UUID | No | - | The ID of the aggregate instance. |
| event_type | VARCHAR(255) | No | - | The specific type of the event (e.g., 'CorrespondentCreated'). |
| event_data | JSONB | No | - | The payload of the event. |
| metadata | JSONB | Yes | - | Additional metadata, such as correlation ID or causation ID. |
| occurred_at | TIMESTAMPTZ | No | - | Timestamp when the event was created. |
| sequence_number | BIGSERIAL | No | - | A globally ordered sequence number for events. |
| version | INTEGER | No | - | The version of the aggregate after this event was applied. |


#### Indexes

- **idx_domain_events_aggregate_id** on (aggregate_id) - To query all events for a specific aggregate instance.
- **idx_domain_events_occurred_at** on (occurred_at) - To query events by time, useful for replay or auditing.

#### Constraints

- **pk_domain_events** (PRIMARY KEY): id
- **uq_domain_events_aggregate_version** (UNIQUE): aggregate_id, version


**DDL:** See migration files in `database-schemas/correspondent_master_db/`

---

### rma_agreements

**Database:** rma_management_db  
**Microservice:** rma-management-api  
**Aggregate:** RelationshipManagementAgreement  
**Columns:** 10  
**Indexes:** 0  
**Constraints:** 4  
**Audit Columns:** Yes ✅  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| id | UUID | No | PK | Internal unique identifier for the RMA agreement. |
| correspondent_bic | VARCHAR(11) | No | UQ | The BIC of the correspondent bank this agreement applies to. |
| status | VARCHAR(20) | No | - | Status of the overall agreement (e.g., ACTIVE, PENDING_APPROVAL). |
| effective_from_date | DATE | No | - | The date from which this agreement is valid. |
| effective_to_date | DATE | Yes | - | The date until which this agreement is valid (NULL for indefinite). |
| created_at | TIMESTAMPTZ | No | - | Timestamp of record creation. |
| updated_at | TIMESTAMPTZ | No | - | Timestamp of last record update. |
| created_by | VARCHAR(255) | Yes | - | Identifier of the user/system that created the record. |
| updated_by | VARCHAR(255) | Yes | - | Identifier of the user/system that last updated the record. |
| version | INTEGER | No | - | Version number for optimistic locking. |


#### Indexes

No indexes defined

#### Constraints

- **pk_rma_agreements** (PRIMARY KEY): id
- **uq_rma_agreements_correspondent_bic** (UNIQUE): correspondent_bic
- **chk_rma_agreements_bic_format** (CHECK): correspondent_bic ~ '^[A-Z]{4}[A-Z]{2}[A-Z0-9]{2}([A-Z0-9]{3})?$'
- **chk_rma_agreements_date_range** (CHECK): effective_to_date IS NULL OR effective_to_date >= effective_from_date


**DDL:** See migration files in `database-schemas/rma_management_db/`

---

### authorized_messages

**Database:** rma_management_db  
**Microservice:** rma-management-api  
**Aggregate:** RelationshipManagementAgreement  
**Columns:** 9  
**Indexes:** 1  
**Constraints:** 3  
**Audit Columns:** Yes ✅  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| id | UUID | No | PK | Internal unique identifier for the authorization record. |
| rma_agreement_id | UUID | No | - | Foreign key linking to the parent RMA agreement. |
| message_type | VARCHAR(20) | No | - | The SWIFT message type (e.g., 'MT103', 'pacs.008'). |
| direction | VARCHAR(10) | No | - | The direction of the message flow (SEND, RECEIVE). |
| service_type | VARCHAR(50) | Yes | - | Optional SWIFT service type (e.g., 'swift.finplus'). |
| is_enabled | BOOLEAN | No | - | Flag indicating if this specific authorization is active. |
| created_at | TIMESTAMPTZ | No | - | Timestamp of record creation. |
| updated_at | TIMESTAMPTZ | No | - | Timestamp of last record update. |
| version | INTEGER | No | - | Version number for optimistic locking. |


#### Indexes

- **idx_authorized_messages_query** on (rma_agreement_id, message_type, direction) - Optimizes the primary query pattern from GET /api/v1/rma/authorizations.

#### Constraints

- **pk_authorized_messages** (PRIMARY KEY): id
- **uq_authorized_messages_rule** (UNIQUE): rma_agreement_id, message_type, direction
- **chk_authorized_messages_direction** (CHECK): direction IN ('SEND', 'RECEIVE')


**DDL:** See migration files in `database-schemas/rma_management_db/`

---

### domain_events

**Database:** rma_management_db  
**Microservice:** rma-management-api  
**Aggregate:** N/A (Cross-cutting concern)  
**Columns:** 0  
**Indexes:** 0  
**Constraints:** 0  
**Audit Columns:** No ⚠️  
**Primary Key:** No ⚠️

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|



#### Indexes

No indexes defined

#### Constraints

No constraints defined


**DDL:** See migration files in `database-schemas/rma_management_db/`

---

### ssi_read_model

**Database:** ssi_read_db  
**Microservice:** ssi-query-api  
**Aggregate:** StandardSettlementInstruction  
**Columns:** 10  
**Indexes:** 1  
**Constraints:** 2  
**Audit Columns:** Yes ✅  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| id | UUID | No | PK | Unique identifier of the SSI, matching the command-side ID. |
| counterparty_id | VARCHAR(50) | No | - | Business identifier for the counterparty. |
| currency | CHAR(3) | No | - | ISO 4217 currency code. |
| payment_product | VARCHAR(50) | No | - | The payment product or scheme this SSI applies to (e.g., 'CROSS_BORDER', 'SEPA'). |
| status | VARCHAR(20) | No | - | Status of the SSI (e.g., ACTIVE, INACTIVE, PENDING). |
| effective_from_date | DATE | No | - | Date from which this SSI is valid. |
| effective_to_date | DATE | Yes | - | Date until which this SSI is valid (NULL for indefinite). |
| routing_details | JSONB | No | - | Denormalized JSON object containing the full routing path, including intermediary and beneficiary bank details. |
| created_at | TIMESTAMPTZ | No | - | Timestamp of original SSI creation. |
| updated_at | TIMESTAMPTZ | No | - | Timestamp of the last update to this read model record. |


#### Indexes

- **idx_ssi_read_model_active_lookup** on (counterparty_id, currency, payment_product) - Highly optimized partial index for the primary query pattern GET /api/v1/ssis/active, which is critical for performance (NFR-005).

#### Constraints

- **pk_ssi_read_model** (PRIMARY KEY): id
- **chk_ssi_read_model_status** (CHECK): status IN ('ACTIVE', 'INACTIVE', 'PENDING_VALIDATION', 'REVOKED')


**DDL:** See migration files in `database-schemas/ssi_read_db/`

---

### standard_settlement_instructions

**Database:** ssi_write_db  
**Microservice:** ssi-command-api  
**Aggregate:** StandardSettlementInstruction  
**Columns:** 13  
**Indexes:** 0  
**Constraints:** 3  
**Audit Columns:** Yes ✅  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| id | UUID | No | PK | Internal unique identifier for the SSI. |
| counterparty_id | VARCHAR(50) | No | - | Business identifier for the counterparty. |
| currency | CHAR(3) | No | - | ISO 4217 currency code. |
| payment_product | VARCHAR(50) | No | - | The payment product or scheme this SSI applies to. |
| status | VARCHAR(20) | No | - | Lifecycle status of the SSI. |
| effective_from_date | DATE | No | - | Date from which this SSI is valid. |
| effective_to_date | DATE | Yes | - | Date until which this SSI is valid. |
| beneficiary_details | JSONB | No | - | Structured details of the final beneficiary bank and account. |
| created_at | TIMESTAMPTZ | No | - | Timestamp of record creation. |
| updated_at | TIMESTAMPTZ | No | - | Timestamp of last record update. |

*... and 3 more columns*

#### Indexes

No indexes defined

#### Constraints

- **pk_ssis** (PRIMARY KEY): id
- **uq_ssis_business_key** (UNIQUE): counterparty_id, currency, payment_product
- **chk_ssis_status** (CHECK): status IN ('ACTIVE', 'INACTIVE', 'PENDING_VALIDATION', 'REVOKED')


**DDL:** See migration files in `database-schemas/ssi_write_db/`

---

### ssi_routing_intermediaries

**Database:** ssi_write_db  
**Microservice:** ssi-command-api  
**Aggregate:** StandardSettlementInstruction  
**Columns:** 7  
**Indexes:** 1  
**Constraints:** 2  
**Audit Columns:** Yes ✅  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| id | UUID | No | PK | Unique identifier for this step in the routing path. |
| ssi_id | UUID | No | - | Foreign key to the parent SSI. |
| sequence_order | INT | No | - | The order of this intermediary in the payment chain (1, 2, ...). |
| intermediary_bic | VARCHAR(11) | No | - | The BIC of the intermediary bank. |
| account_number | VARCHAR(34) | Yes | - | The account number at the intermediary bank. |
| intermediary_details | JSONB | Yes | - | Additional structured details for the intermediary. |
| created_at | TIMESTAMPTZ | No | - | Timestamp of record creation. |


#### Indexes

- **idx_ssi_routing_intermediaries_ssi_id** on (ssi_id) - To quickly retrieve all intermediaries for a given SSI.

#### Constraints

- **pk_ssi_routing_intermediaries** (PRIMARY KEY): id
- **uq_ssi_routing_intermediaries_order** (UNIQUE): ssi_id, sequence_order


**DDL:** See migration files in `database-schemas/ssi_write_db/`

---

### domain_events

**Database:** ssi_write_db  
**Microservice:** ssi-command-api  
**Aggregate:** N/A (Cross-cutting concern)  
**Columns:** 0  
**Indexes:** 0  
**Constraints:** 0  
**Audit Columns:** No ⚠️  
**Primary Key:** No ⚠️

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|



#### Indexes

No indexes defined

#### Constraints

No constraints defined


**DDL:** See migration files in `database-schemas/ssi_write_db/`

---

### nostro_accounts

**Database:** nostro_account_db  
**Microservice:** nostro-account-api  
**Aggregate:** NostroAccount  
**Columns:** 11  
**Indexes:** 1  
**Constraints:** 4  
**Audit Columns:** Yes ✅  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| id | UUID | No | PK | Internal unique identifier for the Nostro account. |
| correspondent_id | UUID | No | - | Reference to the correspondent bank where the account is held. |
| account_number | VARCHAR(34) | No | - | The account number (IBAN or proprietary format). |
| currency | CHAR(3) | No | - | ISO 4217 currency code of the account. |
| account_type | VARCHAR(20) | No | - | Type of account (e.g., NOSTRO, VOSTRO). |
| status | VARCHAR(20) | No | - | Lifecycle status of the account (e.g., ACTIVE, CLOSED). |
| created_at | TIMESTAMPTZ | No | - | Timestamp of record creation. |
| updated_at | TIMESTAMPTZ | No | - | Timestamp of last record update. |
| created_by | VARCHAR(255) | Yes | - | Identifier of the user/system that created the record. |
| updated_by | VARCHAR(255) | Yes | - | Identifier of the user/system that last updated the record. |

*... and 1 more columns*

#### Indexes

- **idx_nostro_accounts_correspondent_id** on (correspondent_id) - To efficiently find all accounts held at a specific correspondent.

#### Constraints

- **pk_nostro_accounts** (PRIMARY KEY): id
- **uq_nostro_accounts_number_currency** (UNIQUE): account_number, currency
- **chk_nostro_accounts_type** (CHECK): account_type IN ('NOSTRO', 'VOSTRO', 'LORO')
- **chk_nostro_accounts_status** (CHECK): status IN ('ACTIVE', 'PENDING_CLOSURE', 'CLOSED')


**DDL:** See migration files in `database-schemas/nostro_account_db/`

---

### statement_rules

**Database:** nostro_account_db  
**Microservice:** nostro-account-api  
**Aggregate:** NostroAccount  
**Columns:** 9  
**Indexes:** 0  
**Constraints:** 2  
**Audit Columns:** Yes ✅  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| id | UUID | No | PK | Unique identifier for the rule. |
| nostro_account_id | UUID | No | - | Foreign key to the parent Nostro account. |
| rule_type | VARCHAR(50) | No | - | Type of rule (e.g., 'MT210_THRESHOLD'). |
| threshold_amount | DECIMAL(19, 4) | Yes | - | The monetary amount that triggers the rule. |
| threshold_currency | CHAR(3) | Yes | - | The currency of the threshold amount. |
| is_enabled | BOOLEAN | No | - | Flag indicating if the rule is active. |
| created_at | TIMESTAMPTZ | No | - | Timestamp of record creation. |
| updated_at | TIMESTAMPTZ | No | - | Timestamp of last record update. |
| version | INTEGER | No | - | Version number for optimistic locking. |


#### Indexes

No indexes defined

#### Constraints

- **pk_statement_rules** (PRIMARY KEY): id
- **uq_statement_rules_account_type** (UNIQUE): nostro_account_id, rule_type


**DDL:** See migration files in `database-schemas/nostro_account_db/`

---

### domain_events

**Database:** nostro_account_db  
**Microservice:** nostro-account-api  
**Aggregate:** N/A (Cross-cutting concern)  
**Columns:** 0  
**Indexes:** 0  
**Constraints:** 0  
**Audit Columns:** No ⚠️  
**Primary Key:** No ⚠️

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|



#### Indexes

No indexes defined

#### Constraints

No constraints defined


**DDL:** See migration files in `database-schemas/nostro_account_db/`

---

### clearing_participants_read_model

**Database:** clearing_directory_read_db  
**Microservice:** clearing-directory-query-api  
**Aggregate:** ClearingSystemParticipant  
**Columns:** 6  
**Indexes:** 1  
**Constraints:** 1  
**Audit Columns:** No ⚠️  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| system_name | VARCHAR(50) | No | PK | The name of the clearing system (e.g., 'TARGET2', 'FEDWIRE'). |
| participant_bic | VARCHAR(11) | No | PK | The BIC of the participant in the clearing system. |
| participant_name | VARCHAR(255) | No | - | The legal name of the participant. |
| status | VARCHAR(20) | No | - | The status of the participant (e.g., 'ACTIVE', 'SUSPENDED'). |
| participant_details | JSONB | Yes | - | Additional system-specific details for the participant. |
| last_synced_at | TIMESTAMPTZ | No | - | Timestamp of the last update from the source. |


#### Indexes

- **idx_clearing_participants_read_model_bic** on (participant_bic) - To allow searching for a participant across all clearing systems.

#### Constraints

- **pk_clearing_participants_read_model** (PRIMARY KEY): system_name, participant_bic


**DDL:** See migration files in `database-schemas/clearing_directory_read_db/`

---

### clearing_participants

**Database:** clearing_directory_write_db  
**Microservice:** clearing-directory-command-api  
**Aggregate:** ClearingSystemParticipant  
**Columns:** 8  
**Indexes:** 0  
**Constraints:** 1  
**Audit Columns:** Yes ✅  
**Primary Key:** Yes ✅

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|
| system_name | VARCHAR(50) | No | PK | The name of the clearing system. |
| participant_bic | VARCHAR(11) | No | PK | The BIC of the participant. |
| participant_name | VARCHAR(255) | No | - | The legal name of the participant. |
| status | VARCHAR(20) | No | - | The status of the participant. |
| participant_details | JSONB | Yes | - | Additional system-specific details. |
| created_at | TIMESTAMPTZ | No | - | Timestamp of record creation. |
| updated_at | TIMESTAMPTZ | No | - | Timestamp of last record update. |
| version | INTEGER | No | - | Version number for optimistic locking. |


#### Indexes

No indexes defined

#### Constraints

- **pk_clearing_participants** (PRIMARY KEY): system_name, participant_bic


**DDL:** See migration files in `database-schemas/clearing_directory_write_db/`

---

### domain_events

**Database:** clearing_directory_write_db  
**Microservice:** clearing-directory-command-api  
**Aggregate:** N/A (Cross-cutting concern)  
**Columns:** 0  
**Indexes:** 0  
**Constraints:** 0  
**Audit Columns:** No ⚠️  
**Primary Key:** No ⚠️

#### Columns

| Name | Type | Nullable | Key | Description |
|------|------|----------|-----|-------------|



#### Indexes

No indexes defined

#### Constraints

No constraints defined


**DDL:** See migration files in `database-schemas/clearing_directory_write_db/`

---


## Indexes

### Index Catalog

- **idx_correspondents_status** on `correspondents` (status) [btree]  
  *Reason:* To efficiently query correspondents by their lifecycle status.

- **idx_correspondents_country_code** on `correspondents` (country_code) [btree]  
  *Reason:* To efficiently query correspondents by country.

- **idx_domain_events_aggregate_id** on `domain_events` (aggregate_id) [btree]  
  *Reason:* To query all events for a specific aggregate instance.

- **idx_domain_events_occurred_at** on `domain_events` (occurred_at) [btree]  
  *Reason:* To query events by time, useful for replay or auditing.

- **idx_authorized_messages_query** on `authorized_messages` (rma_agreement_id, message_type, direction) [btree]  
  *Reason:* Optimizes the primary query pattern from GET /api/v1/rma/authorizations.

- **idx_ssi_read_model_active_lookup** on `ssi_read_model` (counterparty_id, currency, payment_product) [btree]  
  *Reason:* Highly optimized partial index for the primary query pattern GET /api/v1/ssis/active, which is critical for performance (NFR-005).

- **idx_ssi_routing_intermediaries_ssi_id** on `ssi_routing_intermediaries` (ssi_id) [btree]  
  *Reason:* To quickly retrieve all intermediaries for a given SSI.

- **idx_nostro_accounts_correspondent_id** on `nostro_accounts` (correspondent_id) [btree]  
  *Reason:* To efficiently find all accounts held at a specific correspondent.

- **idx_clearing_participants_read_model_bic** on `clearing_participants_read_model` (participant_bic) [btree]  
  *Reason:* To allow searching for a participant across all clearing systems.

### Index Types Used





### Index Best Practices

1. **Foreign Keys:** Always indexed for join performance
2. **Query Patterns:** Indexes match API query parameters
3. **Composite Indexes:** Multi-column WHERE clauses use composite indexes
4. **Partial Indexes:** For common filters (e.g., WHERE status = 'ACTIVE')
5. **Covering Indexes:** Include frequently selected columns

---

## Data Types

### Data Types Used

- **BIGSERIAL**
- **BOOLEAN**
- **CHAR**
- **DATE**
- **DECIMAL**
- **INT**
- **INTEGER**
- **JSONB**
- **TIMESTAMPTZ**
- **UUID**
- **VARCHAR**

### Type Guidelines

**For Money:**
```sql
DECIMAL(19,4)  -- NEVER use FLOAT or REAL for money!
```

**For Identifiers:**
```sql
UUID DEFAULT gen_random_uuid()
```

**For Timestamps:**
```sql
TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
```

**For BIC Codes:**
```sql
VARCHAR(11) CHECK (bic ~ '^[A-Z]{6}[A-Z0-9]{2}([A-Z0-9]{3})?$')
```

**For LEI Codes:**
```sql
VARCHAR(20) CHECK (lei ~ '^[0-9A-Z]{20}$')
```

**For Flexible Data:**
```sql
JSONB  -- Better than TEXT for structured nested data
```

---

## Storage Estimates

### Initial Storage Requirements


- **Total Initial Size:** 6.2 GB
- **Annual Growth:** 0.8 GB/year
- **3-Year Projection:** 8.5 GB

### Capacity Planning

**Hardware Recommendations (3-year horizon):**
- **Storage:** 17 GB (2x for overhead + backups)
- **RAM:** 3 GB (25% of data for buffer pool)
- **IOPS:** Depends on workload (SSD recommended)

**Cloud Estimates (AWS RDS):**
- **Instance Type:** db.r6g.xlarge (4 vCPU, 32 GB RAM) or larger
- **Storage:** 17 GB General Purpose SSD (gp3)
- **Cost Estimate:** ~$500-800/month per database


---

## Migration Scripts

### Flyway Migration Strategy

All database changes are version-controlled using Flyway migrations.

**Naming Convention:** `V{version}__{description}.sql`

**Example:**
```
V1__create_correspondents_table.sql
V2__add_correspondent_indexes.sql
V3__create_contact_details_table.sql
V4__add_audit_triggers.sql
```

### Migration Files by Database

#### correspondent_master_db

- **V1__create_audit_function.sql**: Create a reusable function to update the updated_at timestamp.
- **V2__create_correspondents_table.sql**: Create the main table for correspondent master data.
- **V3__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

#### rma_management_db

- **V1__create_rma_tables.sql**: Create tables for RMA agreements and authorized messages.
- **V2__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

#### ssi_read_db

- **V1__create_ssi_read_model_table.sql**: Create the denormalized read model for SSIs.

#### ssi_write_db

- **V1__create_ssi_tables.sql**: Create normalized tables for the SSI aggregate.
- **V2__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

#### nostro_account_db

- **V1__create_nostro_tables.sql**: Create tables for Nostro accounts and their statement rules.
- **V2__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

#### N/A

No migrations defined

#### clearing_directory_read_db

- **V1__create_clearing_participants_read_model.sql**: Create the denormalized read model for clearing participants.

#### clearing_directory_write_db

- **V1__create_clearing_participants_table.sql**: Create the transactional table for clearing participants.
- **V2__create_domain_events_table.sql**: Create the outbox table for reliable event publishing.

#### N/A

No migrations defined

#### N/A

No migrations defined

#### N/A

No migrations defined

#### N/A (Oracle)

No migrations defined

#### N/A (AWS S3)

No migrations defined


### Running Migrations

**Using Flyway CLI:**
```bash
flyway -url=jdbc:postgresql://localhost:5432/correspondent_directory_db \
       -user=postgres \
       -password=your_password \
       -locations=filesystem:./migrations \
       migrate
```

**Using Spring Boot:**
```yaml
spring:
  flyway:
    enabled: true
    locations: classpath:db/migration
    baseline-on-migrate: true
```

**Using Docker:**
```bash
docker run --rm \
  -v $(pwd)/migrations:/flyway/sql \
  flyway/flyway \
  -url=jdbc:postgresql://db:5432/mydb \
  -user=postgres \
  migrate
```

---

## Best Practices

### 1. Database Per Service

Each microservice owns its database:
- ✅ **Independent deployments** (no coordination needed)
- ✅ **Technology flexibility** (could use different DB per service)
- ✅ **Failure isolation** (one DB down doesn't affect others)
- ❌ **No cross-database joins** (use APIs or events instead)

### 2. Audit Trail

All tables include audit columns:
```sql
created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
created_by VARCHAR(255),
updated_by VARCHAR(255)
```

Trigger automatically updates `updated_at`:
```sql
CREATE TRIGGER trg_table_updated_at
    BEFORE UPDATE ON table_name
    FOR EACH ROW
    EXECUTE FUNCTION update_updated_at_column();
```

### 3. Optimistic Locking

All tables include version column:
```sql
version INTEGER NOT NULL DEFAULT 0
```

Application increments version on update:
```sql
UPDATE correspondents 
SET name = 'New Name', version = version + 1
WHERE id = '...' AND version = 5;  -- Fails if version changed
```

### 4. Constraints for Data Integrity

Use database constraints:
```sql
-- Format validation
CONSTRAINT chk_bic_format CHECK (bic ~ '^[A-Z]{6}...')

-- Business rules
CONSTRAINT chk_amount_positive CHECK (amount > 0)

-- Reference integrity
CONSTRAINT fk_correspondent FOREIGN KEY (correspondent_id) 
    REFERENCES correspondents(id) ON DELETE CASCADE
```

### 5. Indexes for Performance

Index based on query patterns:
```sql
-- Single column (for WHERE/ORDER BY)
CREATE INDEX idx_correspondent_country ON correspondents(country);

-- Composite (for multi-column WHERE)
CREATE INDEX idx_correspondent_country_status 
    ON correspondents(country, status);

-- Partial (for common filters)
CREATE INDEX idx_correspondent_active 
    ON correspondents(country) WHERE status = 'ACTIVE';
```

### 6. Event Sourcing

For services publishing events, include event store:
```sql
CREATE TABLE domain_events (
    id UUID PRIMARY KEY,
    aggregate_type VARCHAR(100),
    aggregate_id UUID,
    event_type VARCHAR(255),
    event_data JSONB,
    occurred_at TIMESTAMP,
    sequence_number BIGSERIAL
);
```

---

## Backup & Recovery

### Backup Strategy

**PostgreSQL Backups:**
- **Daily:** Full backup (pg_dump)
- **Hourly:** WAL archiving (continuous)
- **Retention:** 30 days
- **Location:** S3 bucket (encrypted)

**Backup Command:**
```bash
pg_dump -h localhost -U postgres correspondent_directory_db \
  | gzip > backup_$(date +%Y%m%d).sql.gz
```

### Recovery

**Point-in-Time Recovery (PITR):**
```bash
# Restore base backup
pg_restore -d correspondent_directory_db backup_20241127.sql

# Replay WAL logs to specific time
recovery_target_time = '2024-11-27 10:30:00'
```

---

## Monitoring

### Key Metrics

- **Connection Pool:** Active connections, pool size
- **Query Performance:** Slow query log (> 1s)
- **Index Usage:** Unused indexes, missing indexes
- **Table Bloat:** Dead tuples, vacuum needed
- **Replication Lag:** For read replicas

### Queries for Monitoring

**Find Slow Queries:**
```sql
SELECT query, calls, total_time, mean_time
FROM pg_stat_statements
ORDER BY mean_time DESC
LIMIT 10;
```

**Find Unused Indexes:**
```sql
SELECT schemaname, tablename, indexname
FROM pg_stat_user_indexes
WHERE idx_scan = 0;
```

**Find Missing Indexes:**
```sql
SELECT schemaname, tablename, attname
FROM pg_stats
WHERE n_distinct > 100
  AND correlation < 0.1;
```

---

## Next Steps

1. **Review Schemas:** See `database-schemas/` folder for complete DDL
2. **Set Up Databases:** Create PostgreSQL databases per microservice
3. **Run Migrations:** Use Flyway to create tables
4. **Configure Backups:** Set up automated backups
5. **Set Up Monitoring:** Track performance metrics

---

## Support

For database questions:
- **DDL Scripts:** See `database-schemas/` folder
- **Migration Files:** Flyway scripts in each service folder
- **Contact:** architecture@yourbank.com

---

*This database catalog was auto-generated from the Legacy System Modernization Pipeline.*
