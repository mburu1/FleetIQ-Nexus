# System Design & Data Flow

## GPS Telemetry Ingestion Pipeline
To handle high-throughput telemetry without blocking the API, we utilize an asynchronous ingestion pipeline:

1. **Ingestion**: GPS providers push data to `POST /api/v1/telemetry`.
2. **Validation & Queueing**: The API validates the payload using Zod and pushes it to the `telemetry.ingest` Redis queue. The API returns `202 Accepted` immediately.
3. **Processing (Telemetry Worker)**: 
   - Consumes from `telemetry.ingest`.
   - Normalizes data across different provider formats.
   - Calculates derived events (e.g., harsh braking, geofence violations).
   - Persists to PostgreSQL.
4. **Real-time Broadcasting**: The worker publishes live updates to a Redis Pub/Sub channel, which the WebSocket gateway forwards to connected Next.js clients.

## Integration Abstraction
External GPS providers (Wialon, etc.) are integrated via the Adapter Pattern. Each provider implements a standard `TelemetryProvider` interface, ensuring the core domain remains agnostic to vendor-specific payloads.