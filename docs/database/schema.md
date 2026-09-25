# Database Schema & Design

## Core Entities
The schema is designed around strict multi-tenancy. Every tenant-scoped table includes a `tenantId` foreign key.

### Hierarchy
- **Tenant**: The root isolation boundary.
  - **User**: Platform users mapped to tenants.
  - **Vehicle**: Physical assets.
    - **Device**: Telemetry hardware mapped to vehicles.
      - **Telemetry**: High-volume time-series data.
  - **Driver**: Personnel operating vehicles.
  - **Geofence**: Spatial boundaries for alerts.
  - **MaintenanceRecord**: Service and repair history.
  - **Alert**: System-generated notifications.

## Telemetry Data Strategy
Telemetry data requires special handling due to high write volume.
- **Partitioning**: The `Telemetry` table is partitioned by `timestamp` (monthly) to maintain query performance and simplify data retention policies.
- **Batch Inserts**: Workers batch insert telemetry records to minimize database round-trips.