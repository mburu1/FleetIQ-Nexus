# Tenant Isolation Strategy

Tenant isolation is a first-class architectural concern in FleetIQ Nexus. We employ a **Shared Database, Shared Schema** approach with strict application-level enforcement.

## Enforcement Mechanisms
1. **Context Injection**: The Fastify authentication middleware extracts the `tenantId` from the JWT and attaches it to the request context (`request.tenantId`).
2. **Prisma Middleware / Extensions**: A global Prisma extension intercepts all queries. If a query targets a tenant-scoped model and lacks a `tenantId` filter, the extension automatically injects `where: { tenantId: request.tenantId }`.
3. **Mutation Guards**: Create/Update operations automatically inject the `tenantId` into the payload before database insertion.

## Edge Cases & Mitigations
- **Cross-Tenant Data Leaks**: Prevented by the Prisma extension. Even if a developer forgets to filter by `tenantId`, the extension enforces it.
- **Background Jobs**: Workers processing tenant-specific jobs (e.g., analytics) must explicitly pass the `tenantId` in the job payload and use it to initialize the Prisma client context.