# Design Doc: Personal Trend Dashboard

## 1. Project Summary
Build a personal dashboard that tracks trend datasets (for example stocks and AI model benchmark metrics), with time-series charts and user-defined alerts.

## 2. Users and Core Use Cases
- User creates watchlists for selected metrics/datasets.
- System ingests data on a schedule from external providers.
- User views historical trend charts and comparisons.
- User configures threshold alerts.
- User receives notifications when thresholds are crossed.

## 3. Proposed Architecture
- `frontend`: Dashboard UI with chart panels and watchlist management.
- `api`: Node.js/Express API for watchlist and chart queries.
- `worker`: Scheduler + ingestion pipeline + alert evaluation.
- `postgres`: Stores normalized time-series data and alert history.
- `ingestion-cache` (optional): in-memory cache for provider throttling.

## 4. Data Model (Initial)
- `users`
- `watchlists`
- `watchlist_items`
- `data_providers`
- `data_points`
- `alerts`
- `alert_events`
- `ingestion_runs`

## 5. Course Requirements Plan

| Requirement | Implementation Plan | Evidence/Demo |
|---|---|---|
| Docker containerization | Dockerfiles for frontend, api, worker. | Images built and launched successfully. |
| Docker Compose local setup | Compose with postgres + worker + api + frontend. | Local ingestion and charting runbook. |
| PostgreSQL persistence | Normalized time-series and watchlist data stored in Postgres. | Query history remains after restart. |
| Persistent storage | Postgres PVC on DigitalOcean Block Storage. | Restart/redeploy keeps historical data. |
| Deploy to DO/Fly | Deploy on DigitalOcean Kubernetes. | Running app in DOKS cluster. |
| Orchestration (K8s/Swarm) | Kubernetes Deployments/Services/PVC with autoscaling for api/worker. | HPA demo under synthetic load. |
| Monitoring/observability | Monitor ingestion failures, provider latency, queue lag, API errors. | Alert trigger on ingestion failure. |
| At least 2 advanced features | External data integration, autoscaling/high availability, CI/CD, alerting workflows. | End-to-end ingestion and alert demo. |

## 6. Advanced Features (Selected)
- Integration with external services: ingest data from one stock API and one AI benchmark source.
- Auto-scaling and high availability: HPA for API/worker plus multiple replicas.
- CI/CD pipeline: test/build/deploy via GitHub Actions.
- Monitoring and alerting: ingestion health and threshold alert events.

## 7. API Surface (MVP)
- `POST /auth/register`, `POST /auth/login`
- `GET/POST /watchlists`
- `POST /watchlists/:id/items`
- `GET /series?itemId=&range=`
- `POST /alerts`
- `GET /alerts/events`

## 8. Delivery Plan (8 Weeks)
1. Week 1: pick data providers, schema, and API contract.
2. Week 2: auth, watchlist CRUD, and base UI.
3. Week 3: ingestion worker and normalized storage.
4. Week 4: chart endpoints and threshold alert logic.
5. Week 5: Kubernetes deployment with PVC and initial HPA.
6. Week 6: monitoring dashboards and failure alerts.
7. Week 7: CI/CD and resilience improvements (retry/backoff).
8. Week 8: provider fallback tests and demo script.

## 9. Risks and Mitigations
- API rate limits: cache, backoff, and reduced polling frequency.
- Provider schema changes: adapter layer per provider.
- Scope risk: start with one dataset type, then add second source.
