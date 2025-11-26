# Developer Guide: KRS Correspondentbanking system

**Welcome to the KRS Correspondentbanking system development environment!**

This guide will get you up and running with the modernized architecture in under 30 minutes.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Quick Start](#quick-start)
3. [Local Development Setup](#local-development-setup)
4. [Running Services Locally](#running-services-locally)
5. [Testing](#testing)
6. [Debugging](#debugging)
7. [Common Tasks](#common-tasks)
8. [Project Structure](#project-structure)
9. [Development Workflow](#development-workflow)
10. [Troubleshooting](#troubleshooting)

---

## Prerequisites

### Required Software

Install these tools before starting:

- ✅ **Java 21** (for Spring Boot services): [Download](https://adoptium.net/)
- ✅ **Node.js 20+** (for Node.js services): [Download](https://nodejs.org/)
- ✅ **Docker Desktop**: [Download](https://www.docker.com/products/docker-desktop/)
- ✅ **Git**: [Download](https://git-scm.com/downloads)
- ✅ **kubectl** (Kubernetes CLI): [Install Guide](https://kubernetes.io/docs/tasks/tools/)

### Recommended Tools

- **IDE:** IntelliJ IDEA (Java) or VS Code (Node.js/general)
- **API Testing:** Postman or Insomnia
- **Database Client:** DBeaver or pgAdmin
- **Kafka Tool:** AKHQ or Offset Explorer

### Verify Installation

```bash
java --version    # Should show Java 21
node --version    # Should show v20+
docker --version  # Should show Docker 20+
git --version
kubectl version --client
```

---

## Quick Start

**Get running in 5 minutes:**

```bash
# 1. Clone repository
git clone https://github.com/your-org/krs-correspondentbanking-system-modernization.git
cd krs-correspondentbanking-system-modernization

# 2. Start infrastructure (Postgres, Kafka, Redis)
docker-compose up -d

# 3. Wait for services to be ready
docker-compose ps

# 4. Run database migrations
./scripts/run-migrations.sh

# 5. Start a service (example: correspondent-directory-query-api)
cd services/correspondent-directory-query-api
./gradlew bootRun  # Java/Spring Boot
# OR
npm start          # Node.js

# 6. Test API
curl http://localhost:8080/actuator/health
```

**You're running!** 🎉

---

## Local Development Setup

### 1. Clone Repository

```bash
git clone https://github.com/your-org/krs-correspondentbanking-system-modernization.git
cd krs-correspondentbanking-system-modernization
```

### 2. Environment Variables

Copy the example environment file:

```bash
cp .env.example .env
```

Edit `.env` with your local settings:

```bash
# Database
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres

# Kafka
KAFKA_BOOTSTRAP_SERVERS=localhost:9092

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# OAuth (for local testing)
OAUTH_CLIENT_ID=dev-client
OAUTH_CLIENT_SECRET=dev-secret

# Observability
JAEGER_ENDPOINT=http://localhost:14268/api/traces
```

### 3. Start Infrastructure

We use **docker-compose** for local infrastructure:

```bash
docker-compose up -d
```

**What this starts:**
- **PostgreSQL** (13 databases) - Port 5432
- **Kafka + Zookeeper** - Port 9092
- **Redis** - Port 6379
- **Schema Registry** (for Avro) - Port 8081
- **AKHQ** (Kafka UI) - Port 8085
- **Jaeger** (tracing) - Port 16686

**Verify:**
```bash
docker-compose ps
# All services should be "Up"

# Access UIs:
# - AKHQ (Kafka): http://localhost:8085
# - Jaeger (tracing): http://localhost:16686
```

### 4. Create Databases

Run the setup script:

```bash
./scripts/setup-databases.sh
```

This creates 13 PostgreSQL databases:
- `correspondent_master_db`
- `rma_management_db`
- `ssi_read_db`
- `ssi_write_db`
- `nostro_account_db`

### 5. Run Migrations

Run Flyway migrations for all services:

```bash
./scripts/run-migrations.sh
```

Or for a specific service:

```bash
cd services/correspondent-directory-query-api
./gradlew flywayMigrate
```

### 6. Load Test Data (Optional)

```bash
./scripts/load-test-data.sh
```

This loads sample correspondent banks, relationships, etc.

---

## Running Services Locally

### Starting All Services

```bash
./scripts/start-all-services.sh
```

This starts all 13 microservices in the background.

### Starting Individual Services

**Java/Spring Boot services:**

```bash
cd services/correspondent-directory-query-api
./gradlew bootRun
```

**Node.js services:**

```bash
cd services/correspondent-data-gateway
npm install
npm start
```

### Service Ports

- **correspondent-master-data-api**: http://localhost:8080
- **rma-management-api**: http://localhost:8081
- **ssi-query-api**: http://localhost:8082
- **ssi-command-api**: http://localhost:8083
- **nostro-account-api**: http://localhost:8084
- **nostro-statement-worker**: http://localhost:8085
- **clearing-directory-query-api**: http://localhost:8086
- **clearing-directory-command-api**: http://localhost:8087
- **clearing-directory-sync-worker**: http://localhost:8088
- **correspondent-data-gateway**: http://localhost:8089

### Health Checks

Check if a service is running:

```bash
curl http://localhost:8080/actuator/health
```

**Expected response:**
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

## Testing

### Running Tests

**All tests for a service:**

```bash
cd services/correspondent-directory-query-api
./gradlew test  # Java
npm test        # Node.js
```

**Integration tests (with Testcontainers):**

```bash
./gradlew integrationTest
```

**Contract tests (Spring Cloud Contract):**

```bash
./gradlew contractTest
```

### Test Coverage

```bash
./gradlew jacocoTestReport
# Report: build/reports/jacoco/test/html/index.html
```

**Target:** 80% code coverage

### Testing APIs

**Using cURL:**

```bash
# Get correspondent by ID
curl http://localhost:8080/api/v1/correspondents/550e8400-e29b-41d4-a716-446655440000

# Search correspondents
curl "http://localhost:8080/api/v1/correspondents?country=DE&page=0&size=20"

# Create correspondent
curl -X POST http://localhost:8080/api/v1/correspondents \
  -H "Content-Type: application/json" \
  -d '{
    "bic": "DEUTDEFF",
    "name": "Deutsche Bank AG",
    "country": "DE"
  }'
```

**Using Postman:**

1. Import OpenAPI specs from `api-specs/` folder
2. Set `base_url` variable to `http://localhost:8080`
3. Run requests from generated collection

---

## Debugging

### Java Services (IntelliJ IDEA)

1. **Run in Debug Mode:**
   - Open service in IntelliJ
   - Click green bug icon next to `main()` method
   - Or: `./gradlew bootRun --debug-jvm` and attach debugger on port 5005

2. **Set Breakpoints:**
   - Click left gutter next to line number

3. **Debug Remote Service:**
   ```bash
   # Start with debug enabled
   java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar app.jar
   ```
   
   Then in IntelliJ: Run → Attach to Process → Select port 5005

### Node.js Services (VS Code)

**launch.json:**

```json
{
  "type": "node",
  "request": "launch",
  "name": "Debug Service",
  "program": "${workspaceFolder}/src/index.js",
  "envFile": "${workspaceFolder}/.env"
}
```

Press F5 to start debugging.

### Viewing Logs

**Service logs:**
```bash
# Real-time
docker-compose logs -f correspondent-directory-query-api

# Search logs
docker-compose logs | grep ERROR
```

**Kafka messages:**
```bash
# Via AKHQ UI
open http://localhost:8085

# Or via CLI
kafka-console-consumer \
  --bootstrap-server localhost:9092 \
  --topic correspondent.directory.events \
  --from-beginning
```

### Distributed Tracing

Open Jaeger UI: http://localhost:16686

1. Select service from dropdown
2. Click "Find Traces"
3. Click trace to see full request flow across services

---

## Common Tasks

### Adding a New Endpoint

**1. Update API Spec (OpenAPI):**

```yaml
# api-specs/correspondent-directory-query-api.yaml
paths:
  /api/v1/correspondents/search:
    post:
      summary: Advanced correspondent search
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SearchRequest'
```

**2. Generate Code from Spec:**

```bash
./gradlew openApiGenerate
```

**3. Implement Controller:**

```java
@RestController
@RequestMapping("/api/v1/correspondents")
public class CorrespondentController {
    
    @PostMapping("/search")
    public ResponseEntity<Page<CorrespondentDTO>> search(
        @RequestBody SearchRequest request
    ) {
        // Implementation
    }
}
```

**4. Add Tests:**

```java
@SpringBootTest
@AutoConfigureMockMvc
class CorrespondentControllerTest {
    
    @Test
    void testSearch() {
        // Test implementation
    }
}
```

### Adding a New Event

**1. Define Avro Schema:**

```json
// event-schemas/CorrespondentUpdated.avsc
{
  "type": "record",
  "name": "CorrespondentUpdated",
  "namespace": "com.bank.correspondent.events.v1",
  "fields": [
    {"name": "correspondent_id", "type": "string"},
    {"name": "updated_fields", "type": {"type": "array", "items": "string"}}
  ]
}
```

**2. Generate Java Classes:**

```bash
./gradlew generateAvro
```

**3. Publish Event:**

```java
@Service
public class CorrespondentService {
    
    @Autowired
    private KafkaTemplate<String, CorrespondentUpdated> kafka;
    
    public void updateCorrespondent(String id, UpdateRequest req) {
        // Update database
        correspondent.update(req);
        
        // Publish event
        CorrespondentUpdated event = new CorrespondentUpdated(
            id, 
            req.getUpdatedFields()
        );
        kafka.send("correspondent.directory.events", id, event);
    }
}
```

**4. Consume Event:**

```java
@Service
public class CorrespondentEventConsumer {
    
    @KafkaListener(topics = "correspondent.directory.events")
    public void handleUpdated(CorrespondentUpdated event) {
        // Update read model/cache
    }
}
```

### Adding Database Migration

**1. Create Migration File:**

```bash
# Format: V{version}__{description}.sql
touch services/correspondent-directory-query-api/src/main/resources/db/migration/V6__add_email_column.sql
```

**2. Write SQL:**

```sql
-- V6__add_email_column.sql
ALTER TABLE correspondents 
ADD COLUMN email VARCHAR(255);

CREATE INDEX idx_correspondent_email 
ON correspondents(email);
```

**3. Run Migration:**

```bash
./gradlew flywayMigrate
```

### Updating Dependencies

**Java (Gradle):**

```bash
./gradlew dependencyUpdates
# Review: build/dependencyUpdates/report.txt

# Update version in build.gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web:3.2.0'
}
```

**Node.js:**

```bash
npm outdated
npm update
```

---

## Project Structure

```
krs-correspondentbanking-system-modernization/
├── docs/                           # Architecture documentation
│   ├── ARCHITECTURE.md
│   ├── API_CATALOG.md
│   ├── DATABASE_CATALOG.md
│   ├── EVENT_CATALOG.md
│   ├── MICROSERVICES_CATALOG.md
│   ├── bounded-contexts/          # Canvas v5 documents
│   └── diagrams/                  # Context maps
│
├── services/                      # All microservices
│   ├── correspondent-directory-query-api/
│   │   ├── src/
│   │   │   ├── main/
│   │   │   │   ├── java/
│   │   │   │   └── resources/
│   │   │   │       ├── application.yml
│   │   │   │       └── db/migration/    # Flyway migrations
│   │   │   └── test/
│   │   ├── build.gradle
│   │   └── Dockerfile
│   │
│   ├── correspondent-directory-command-api/
│   └── ... (13 total services)
│
├── api-specs/                     # OpenAPI specifications
│   ├── correspondent-directory-query-api.yaml
│   └── ...
│
├── database-schemas/              # SQL DDL scripts
│   ├── correspondent_directory_db/
│   │   └── migrations/
│   └── ...
│
├── event-schemas/                 # Avro schemas
│   ├── CorrespondentCreated.avsc
│   └── ...
│
├── infrastructure/                # Infrastructure as Code
│   ├── kubernetes/               # K8s manifests
│   │   ├── namespaces/
│   │   ├── services/
│   │   └── ingress/
│   ├── terraform/                # Cloud infrastructure
│   └── helm/                     # Helm charts
│
├── scripts/                      # Utility scripts
│   ├── setup-databases.sh
│   ├── run-migrations.sh
│   ├── start-all-services.sh
│   └── load-test-data.sh
│
├── docker-compose.yml            # Local infrastructure
├── .env.example                  # Environment template
└── README.md                     # Project overview
```

---

## Development Workflow

### 1. Pick a Task

From Jira backlog or GitHub issues:

```bash
# Example: "Add BIC validation endpoint"
```

### 2. Create Branch

```bash
git checkout -b feature/add-bic-validation
```

**Branch naming:**
- `feature/description` - New features
- `fix/description` - Bug fixes
- `refactor/description` - Code refactoring
- `docs/description` - Documentation updates

### 3. Implement Changes

1. Update API spec (if adding/changing endpoints)
2. Write failing test
3. Implement feature
4. Make test pass
5. Refactor
6. Add integration tests

### 4. Test Locally

```bash
# Unit tests
./gradlew test

# Integration tests
./gradlew integrationTest

# Manual testing
curl http://localhost:8080/api/v1/correspondents/validate-bic?bic=DEUTDEFF
```

### 5. Commit & Push

```bash
git add .
git commit -m "feat: add BIC validation endpoint

- Added POST /api/v1/correspondents/validate-bic
- Validates BIC format (ISO 9362)
- Returns validation result with error details

Closes #123"

git push origin feature/add-bic-validation
```

**Commit message format:**
- `feat:` New feature
- `fix:` Bug fix
- `refactor:` Code refactoring
- `docs:` Documentation
- `test:` Test changes
- `chore:` Build/config changes

### 6. Create Pull Request

1. Go to GitHub
2. Click "New Pull Request"
3. Select your branch
4. Fill in PR template:
   - What changed
   - Why changed
   - How to test
   - Screenshots (if UI)

### 7. Code Review

Team reviews your PR:
- Code quality
- Test coverage
- Architecture alignment
- Documentation

Address feedback, push updates.

### 8. Merge & Deploy

Once approved:
1. Squash & merge to `main`
2. CI/CD pipeline runs automatically
3. Deploys to staging
4. Run smoke tests
5. Deploy to production (with approval)

---

## Troubleshooting

### Service Won't Start

**Check logs:**
```bash
docker-compose logs service-name
```

**Common issues:**

**"Port already in use":**
```bash
# Find process using port 8080
lsof -i :8080
# Kill process
kill -9 <PID>
```

**"Cannot connect to database":**
```bash
# Check if Postgres is running
docker-compose ps postgres
# Restart if needed
docker-compose restart postgres
```

**"Kafka broker not available":**
```bash
# Check Kafka
docker-compose ps kafka
# View Kafka logs
docker-compose logs kafka
```

### Tests Failing

**"Database tests fail":**
- Check Testcontainers is working
- Verify Docker is running
- Clean test databases: `./gradlew clean`

**"Integration tests timeout":**
- Increase timeout in test config
- Check if required services are running

### Performance Issues

**"Service is slow":**
1. Check database query performance
2. Check cache hit rates (Redis)
3. Look for N+1 query problems
4. Profile with JProfiler/VisualVM

**"High memory usage":**
```bash
# Check Java heap
jmap -heap <PID>
# Take heap dump
jmap -dump:format=b,file=heap.bin <PID>
```

### Can't Find Documentation

**All docs are in:**
- `docs/` folder
- `README.md` (this file)
- `API_CATALOG.md` - All APIs
- `DATABASE_CATALOG.md` - All schemas
- `EVENT_CATALOG.md` - All events
- `MICROSERVICES_CATALOG.md` - All services

---

## Getting Help

### Internal Resources

- **Architecture Docs:** `docs/` folder
- **Wiki:** [Confluence Space](https://wiki.example.com/krs-correspondentbanking-system)
- **API Docs:** Swagger UI at each service `/swagger-ui/`
- **Slack:** `#krs-correspondentbanking-system-dev` channel

### External Resources

- **Spring Boot:** https://spring.io/projects/spring-boot
- **Node.js:** https://nodejs.org/docs
- **Kafka:** https://kafka.apache.org/documentation
- **PostgreSQL:** https://www.postgresql.org/docs
- **BIAN:** https://bian.org

### Contact

- **Tech Lead:** [Name] - [email]
- **Architecture:** architecture@example.com
- **DevOps:** devops@example.com

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for:
- Code style guidelines
- PR process
- Testing requirements
- Documentation standards

---

**Happy coding! 🚀**

*Questions? Ask in `#krs-correspondentbanking-system-dev` Slack channel.*
