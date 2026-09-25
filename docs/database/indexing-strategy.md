# Indexing Strategy

To ensure sub-second query responses for dashboard and API requests, the following PostgreSQL indexes are mandated:

1. **Tenant Isolation Indexes**:
   - Every tenant-scoped table must have a composite index starting with `tenantId`.
   - Example: `CREATE INDEX idx_vehicle_tenant ON vehicles(tenantId, id);`

2. **High-Volume Lookups**:
   - **Telemetry**: `CREATE INDEX idx_telemetry_device_time ON telemetry(deviceId, timestamp DESC);`
   - **Geofences**: PostGIS spatial indexes for geofence intersection queries.

3. **Status & Filtering**:
   - **Alerts**: `CREATE INDEX idx_alert_status ON alerts(tenantId, status, createdAt);`
   - **Maintenance**: `CREATE INDEX idx_maintenance_due ON maintenance_records(tenantId, dueDate) WHERE status != 'COMPLETED';`

## Query Optimization
- Avoid `SELECT *`. Use Prisma's `select` to fetch only required columns.
- Use `EXPLAIN ANALYZE` for all complex reporting queries.
- Implement connection pooling via Prisma's built-in pool configuration.