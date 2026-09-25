# FleetIQ Nexus: Architectural Overview

## System Context
FleetIQ Nexus is a multi-tenant SaaS platform for fleet intelligence and telematics. It ingests high-volume GPS telemetry, processes it asynchronously, and exposes it via REST and WebSockets to a Next.js frontend.

## High-Level Topology
- **Client Layer**: Next.js (React, TypeScript, Tailwind, MapLibre) acting as a Progressive Web App (PWA).
- **API Gateway / Reverse Proxy**: Traefik/Nginx handling TLS termination, routing, and rate limiting.
- **Application Layer**: 
  - `apps/web`: Next.js frontend.
  - `apps/api`: Fastify-based Node.js REST & WebSocket API.
- **Processing Layer**: Background workers (BullMQ) handling telemetry ingestion, analytics calculation, and notifications.
- **Data Layer**: 
  - PostgreSQL (Prisma ORM) for relational domain data.
  - Redis for caching, distributed locks, and job queues.

## Core Principles
1. **Multi-Tenancy**: Tenant isolation is enforced at the application and database query layers.
2. **Modular Monorepo**: Managed via pnpm workspaces and Turborepo for shared types and configurations.
3. **Asynchronous Processing**: High-volume telemetry is decoupled from the synchronous API using Redis queues.
4. **Contract-First**: Strict typing across the stack using shared Zod schemas and TypeScript interfaces.