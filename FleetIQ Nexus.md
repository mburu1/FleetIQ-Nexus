# 🚚 FleetIQ Nexus

> Multi-tenant fleet intelligence and telematics platform built with Next.js, TypeScript, Fastify, PostgreSQL, Prisma and Redis.

FleetIQ Nexus is a production-oriented fleet management and intelligence platform designed for logistics companies, fleet operators and transport businesses.

The platform combines vehicle management, GPS telemetry, driver performance, maintenance intelligence, geospatial tracking, alerts, analytics and operational workflows into a single multi-tenant SaaS platform.

The project is intentionally designed around the technologies used in modern TypeScript/Node.js production environments.

---

## 🎯 Problem

Fleet operators often work with fragmented systems:

* GPS tracking in one platform
* Vehicle information in another
* Driver performance elsewhere
* Maintenance records in spreadsheets
* Alerts delivered through different systems
* Reporting performed manually
* Operational data disconnected from management dashboards

FleetIQ Nexus provides a unified operational platform capable of ingesting telemetry and transforming it into actionable fleet intelligence.

---

# 🧠 Core Capabilities

### Fleet Management

* Vehicle registration
* Vehicle profiles
* Vehicle status
* Vehicle assignments
* Vehicle groups
* Driver assignment
* Vehicle documents
* Insurance and compliance tracking

### GPS & Telematics

* GPS position ingestion
* Vehicle location tracking
* Speed
* Ignition state
* Mileage
* Fuel data
* Geofencing
* Route history
* Device connectivity
* Telemetry events

### Driver Intelligence

* Driver profiles
* Driver-to-vehicle assignments
* Driving behaviour
* Speed violations
* Harsh braking
* Harsh acceleration
* Idling
* Safety events
* Driver performance scoring

### Maintenance

* Maintenance schedules
* Service intervals
* Maintenance history
* Vehicle health
* Fault events
* Preventive maintenance
* Maintenance alerts

### Analytics

* Fleet utilisation
* Vehicle uptime
* Driver performance
* Fuel consumption
* Distance travelled
* Maintenance costs
* Safety incidents
* Operational KPIs

### Notifications

* Vehicle offline
* Overspeed
* Geofence violation
* Maintenance due
* Driver safety event
* Device disconnected

---

# 🏢 Multi-Tenant SaaS

FleetIQ Nexus uses tenant isolation as a first-class architectural concern.

```text
Platform
│
├── Tenant A
│   ├── Users
│   ├── Vehicles
│   ├── Drivers
│   ├── Devices
│   └── Telemetry
│
├── Tenant B
│   ├── Users
│   ├── Vehicles
│   ├── Drivers
│   ├── Devices
│   └── Telemetry
│
└── Tenant C
    ├── Users
    ├── Vehicles
    ├── Drivers
    ├── Devices
    └── Telemetry
```

Every tenant-scoped entity contains a `tenantId`.

Authorization is enforced at the application/service layer rather than relying solely on frontend filtering.

---

# 🏗️ Architecture

FleetIQ Nexus follows a modular architecture.

```text
                         ┌──────────────────────┐
                         │      Next.js Web     │
                         │ React + TypeScript   │
                         └──────────┬───────────┘
                                    │
                              REST / WebSocket
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │    Fastify API       │
                         │     Node.js           │
                         └──────────┬───────────┘
                                    │
                ┌───────────────────┼───────────────────┐
                │                   │                   │
                ▼                   ▼                   ▼
          ┌───────────┐      ┌────────────┐      ┌────────────┐
          │ PostgreSQL│      │   Redis    │      │ External   │
          │  Prisma   │      │ Cache/Jobs │      │ Integrations│
          └───────────┘      └────────────┘      └────────────┘
                                    │
                                    ▼
                             ┌─────────────┐
                             │   Workers   │
                             │ Background  │
                             │ Processing  │
                             └─────────────┘
```

---

# 📦 Repository Structure

```text
fleetiq-nexus/
│
├── apps/
│   │
│   ├── web/
│   │   ├── app/
│   │   ├── components/
│   │   ├── features/
│   │   │   ├── dashboard/
│   │   │   ├── vehicles/
│   │   │   ├── drivers/
│   │   │   ├── tracking/
│   │   │   ├── maintenance/
│   │   │   └── analytics/
│   │   ├── hooks/
│   │   ├── lib/
│   │   ├── services/
│   │   ├── stores/
│   │   ├── types/
│   │   └── middleware.ts
│   │
│   └── api/
│       ├── src/
│       │   ├── modules/
│       │   │   ├── auth/
│       │   │   ├── tenants/
│       │   │   ├── users/
│       │   │   ├── vehicles/
│       │   │   ├── drivers/
│       │   │   ├── tracking/
│       │   │   ├── telemetry/
│       │   │   ├── geofences/
│       │   │   ├── maintenance/
│       │   │   ├── alerts/
│       │   │   ├── analytics/
│       │   │   └── integrations/
│       │   │
│       │   ├── plugins/
│       │   ├── middleware/
│       │   ├── hooks/
│       │   ├── config/
│       │   ├── infrastructure/
│       │   ├── app.ts
│       │   └── server.ts
│       │
│       └── tests/
│
├── workers/
│   ├── telemetry-worker/
│   ├── notification-worker/
│   ├── analytics-worker/
│   ├── maintenance-worker/
│   └── scheduler/
│
├── packages/
│   ├── shared/
│   ├── types/
│   ├── validation/
│   ├── config/
│   └── logger/
│
├── prisma/
│   ├── schema.prisma
│   ├── migrations/
│   └── seed.ts
│
├── infrastructure/
│   ├── docker/
│   ├── nginx/
│   ├── traefik/
│   ├── postgres/
│   └── redis/
│
├── tests/
│   ├── integration/
│   └── e2e/
│
├── docs/
│   ├── architecture/
│   ├── api/
│   ├── database/
│   ├── security/
│   ├── deployment/
│   ├── observability/
│   └── adr/
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       ├── test.yml
│       ├── security.yml
│       └── deploy.yml
│
├── docker-compose.yml
├── docker-compose.prod.yml
├── package.json
├── pnpm-workspace.yaml
├── turbo.json
├── tsconfig.json
├── eslint.config.js
├── prettier.config.js
└── README.md
```

---

# 🛠️ Technology Stack

## Frontend

* Next.js
* React
* TypeScript
* Tailwind CSS
* React Query / TanStack Query
* Zustand
* React Hook Form
* Zod
* MapLibre / Mapbox-compatible mapping
* WebSocket client
* Progressive Web App capabilities

## Backend

* Node.js
* TypeScript
* Fastify
* REST API
* WebSockets
* Zod
* JWT
* RBAC

## Database

* PostgreSQL
* Prisma ORM
* Database migrations
* PostgreSQL indexes
* Query optimisation
* Transactions

## Distributed Processing

* Redis
* BullMQ
* Background workers
* Scheduled jobs
* Retry policies
* Dead-letter handling

## Infrastructure

* Docker
* Docker Compose
* Linux
* Nginx / Traefik
* SSL/TLS
* GitHub Actions
* Cloud deployment

## Testing

* Vitest
* Supertest
* Playwright
* Integration testing
* E2E testing

## Observability

* Structured logging
* OpenTelemetry
* Metrics
* Health checks
* Error tracking
* Request correlation IDs

---

# 🔐 Authentication & Authorization

FleetIQ Nexus implements:

* JWT authentication
* Refresh tokens
* Role-based access control
* Tenant-level authorization
* Permission-based authorization
* API security
* Rate limiting
* Secure HTTP headers
* Input validation
* Audit logging

Example roles:

```text
PlatformAdmin

TenantAdmin
FleetManager
Dispatcher
MaintenanceManager
Driver
Analyst
Viewer
```

---

# 🌍 GPS Architecture

Telemetry providers can submit data through an integration abstraction.

```text
GPS Provider
     │
     ▼
Integration Adapter
     │
     ▼
Telemetry API
     │
     ▼
Redis Queue
     │
     ▼
Telemetry Worker
     │
     ├── Validate
     ├── Normalize
     ├── Persist
     ├── Calculate events
     └── Publish updates
             │
             ▼
       WebSocket Gateway
             │
             ▼
        Live Dashboard
```

This allows additional providers to be introduced without coupling the core domain to a single GPS vendor.

---

# 📡 External Integrations

The integration layer is designed for providers such as:

* Wialon
* GPS tracking platforms
* Mapping providers
* Geocoding services
* IoT platforms
* Email providers
* SMS providers
* Notification services

Integration failures are handled using:

* Timeouts
* Retries
* Exponential backoff
* Circuit breakers
* Structured logging
* Dead-letter queues

---

# ⚡ Redis & Background Jobs

Redis is used for:

* Caching
* Job queues
* Rate limiting
* Distributed locks
* Temporary state
* Real-time event coordination

Example queues:

```text
telemetry.ingest
telemetry.process
notifications.send
analytics.calculate
maintenance.schedule
reports.generate
```

---

# 🗄️ Database Model

Core entities include:

```text
Tenant
 │
 ├── User
 ├── Role
 ├── Vehicle
 │    └── Device
 │         └── Telemetry
 │
 ├── Driver
 ├── Geofence
 ├── MaintenanceRecord
 ├── Alert
 └── AuditLog
```

Important PostgreSQL indexes are designed around:

* `tenantId`
* vehicle lookup
* device lookup
* telemetry timestamps
* driver lookup
* alert status
* maintenance dates

Telemetry data is designed with high-volume ingestion in mind.

---

# 📊 Dashboard

The main dashboard provides:

```text
┌───────────────────────────────────────────────────────────────┐
│ FleetIQ Nexus                              Tenant: Acme Fleet │
├───────────────────────────────────────────────────────────────┤
│                                                               │
│  Vehicles       Active        Alerts        Drivers           │
│    248             231           17            186            │
│                                                               │
├──────────────────────────────┬────────────────────────────────┤
│                              │                                │
│       LIVE FLEET MAP         │       FLEET PERFORMANCE       │
│                              │                                │
│     🚚    🚚                 │   Utilisation     87%          │
│          🚚                  │   Uptime           94%         │
│   🚚                       │   Fuel efficiency  82%          │
│                              │                                │
├──────────────────────────────┴────────────────────────────────┤
│                                                               │
│ Driver Safety        Maintenance         Fleet Utilisation    │
│ ─────────────        ───────────          ────────────────    │
│  ███████████          ████████             ██████████          │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

---

# 📈 Analytics

FleetIQ Nexus provides operational analytics including:

### Vehicle

* Distance
* Utilisation
* Uptime
* Downtime
* Fuel consumption

### Driver

* Safety score
* Speed events
* Harsh braking
* Harsh acceleration
* Idle time

### Fleet

* Cost per kilometre
* Fleet utilisation
* Maintenance expenditure
* Vehicle availability

---

# 🧪 Testing Strategy

Testing follows multiple levels.

```text
             E2E
              │
       Integration
              │
           API Tests
              │
          Unit Tests
              │
        Domain Logic
```

### Unit

* Domain services
* Validation
* Business rules
* Calculations

### Integration

* PostgreSQL
* Redis
* Fastify API
* Prisma

### E2E

* Authentication
* Tenant onboarding
* Vehicle creation
* GPS ingestion
* Dashboard workflows
* Maintenance workflows

---

# 🐳 Docker

Development environment:

```text
Docker Compose
│
├── PostgreSQL
├── Redis
├── API
├── Workers
├── Web
└── Reverse Proxy
```

Production containers are designed to be stateless wherever possible.

---

# 🚀 CI/CD

GitHub Actions pipeline:

```text
Pull Request
     │
     ▼
Lint
     │
     ▼
Type Check
     │
     ▼
Unit Tests
     │
     ▼
Integration Tests
     │
     ▼
Build
     │
     ▼
Security Scan
     │
     ▼
Docker Build
     │
     ▼
Deploy
     │
     ▼
Health Check
```

---

# 🐧 Production Deployment

Target environment:

```text
Linux Server
     │
     ▼
Traefik / Nginx
     │
     ├───────────────┐
     ▼               ▼
 Next.js           Fastify
                     │
             ┌───────┴────────┐
             ▼                ▼
        PostgreSQL          Redis
                              │
                              ▼
                           Workers
```

TLS certificates, domains and routing are handled at the reverse-proxy layer.

---

# 🔎 Observability

The platform includes:

* Structured JSON logging
* Correlation IDs
* Request tracing
* Metrics
* Health endpoints
* Readiness checks
* Liveness checks
* Error monitoring
* Background job monitoring

Example:

```text
Request
  │
  ├── correlationId
  ├── tenantId
  ├── userId
  ├── duration
  └── statusCode
```

Sensitive information is excluded from application logs.

---

# 🔒 Security

Security considerations include:

* Password hashing
* JWT rotation
* RBAC
* Tenant isolation
* Input validation
* SQL injection protection through Prisma
* Rate limiting
* CORS policy
* CSRF protection where applicable
* Secure cookies
* TLS
* Secret management
* Audit logs
* Dependency scanning
* Container security

Secrets are never committed to source control.

---

# 📚 API

Example endpoints:

```http
POST   /api/v1/auth/login
POST   /api/v1/auth/refresh

GET    /api/v1/vehicles
POST   /api/v1/vehicles
GET    /api/v1/vehicles/:id

GET    /api/v1/drivers
POST   /api/v1/drivers

GET    /api/v1/tracking/live
GET    /api/v1/tracking/history

POST   /api/v1/telemetry

GET    /api/v1/geofences
POST   /api/v1/geofences

GET    /api/v1/maintenance
POST   /api/v1/maintenance

GET    /api/v1/analytics/fleet
GET    /api/v1/analytics/drivers

GET    /api/v1/alerts
```

---

# 🧩 Architectural Principles

FleetIQ Nexus follows:

* SOLID
* Clean Architecture principles
* Domain-driven modular boundaries
* Dependency inversion
* Separation of concerns
* Stateless API design
* Twelve-factor application principles
* API versioning
* Contract-first thinking
* Automated testing
* Observability-first production design

---

# 📋 Development Roadmap

## Phase 1 — Foundation

* Monorepo
* Next.js
* Fastify
* PostgreSQL
* Prisma
* Docker
* Authentication

## Phase 2 — Fleet

* Tenants
* Users
* Vehicles
* Drivers
* Devices

## Phase 3 — Telemetry

* GPS ingestion
* Redis queues
* Workers
* Live tracking
* WebSockets

## Phase 4 — Intelligence

* Driver scoring
* Fleet analytics
* Alerts
* Maintenance intelligence

## Phase 5 — Production

* CI/CD
* Reverse proxy
* SSL
* Monitoring
* Backups
* Security hardening

---

# 🎯 Why This Project?

FleetIQ Nexus intentionally demonstrates the engineering capabilities required for modern full-stack SaaS development:

```text
Frontend
    ↓
Next.js + React + TypeScript

Backend
    ↓
Node.js + Fastify + TypeScript

Data
    ↓
PostgreSQL + Prisma

Distributed processing
    ↓
Redis + BullMQ + Workers

Infrastructure
    ↓
Docker + Linux + Nginx/Traefik

Delivery
    ↓
GitHub Actions + Cloud

Production
    ↓
Observability + Security + Reliability
```

---

# 📄 License

MIT License
