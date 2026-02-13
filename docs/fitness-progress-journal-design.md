# Design Doc: Fitness Progress Journal

## 1. Project Summary
Build a personal fitness tracking app where users log workouts and body metrics, then view progress trends over time.

## 2. Users and Core Use Cases
- User creates account and logs in.
- User records workout sessions (exercises, reps, sets, weight, duration).
- User records body metrics (weight, body fat, waist).
- User views trend charts and weekly summaries.
- User receives reminder if no activity is logged for a configured period.

## 3. Proposed Architecture
- `frontend`: Next.js web UI for logging and charts.
- `api`: Node.js/Express REST API + WebSocket endpoint.
- `worker`: Background jobs for reminders and summary generation.
- `postgres`: PostgreSQL database with persistent storage.
- `ingress`: Public routing to frontend/API on DigitalOcean Kubernetes.

## 4. Data Model (Initial)
- `users`
- `workout_sessions`
- `workout_exercises`
- `body_metrics`
- `goals`
- `reminder_preferences`
- `notification_events`

## 5. Course Requirements Plan

| Requirement | Implementation Plan | Evidence/Demo |
|---|---|---|
| Docker containerization | Create separate Dockerfiles for frontend, api, worker. | Build logs + running containers. |
| Docker Compose local setup | `docker-compose.yml` with frontend + api + worker + postgres. | One-command local startup demo. |
| PostgreSQL persistence | All fitness logs stored in normalized Postgres schema. | Query session history after restart. |
| Persistent storage | Use DigitalOcean Block Storage volume for Postgres PVC. | Redeploy/restart and verify data remains. |
| Deploy to DO/Fly | Deploy to DigitalOcean Kubernetes (DOKS). | Public URL + cluster resources screenshot. |
| Orchestration (K8s/Swarm) | Use Kubernetes (Minikube local, DOKS cloud). Deployments + Services + PVC. | `kubectl get` outputs and scaling demo. |
| Monitoring/observability | DO metrics for CPU/memory; app `/health`; error logs in centralized log sink. | Alert trigger and dashboard during demo. |
| At least 2 advanced features | Real-time updates, security/auth, CI/CD, backup. | Feature walkthrough + pipeline runs. |

## 6. Advanced Features (Selected)
- Real-time functionality: WebSocket push when workouts/metrics are added.
- Security enhancements: JWT auth, password hashing, secret management.
- CI/CD: GitHub Actions for test/build/deploy.
- Backup and recovery: nightly Postgres backup job and restore script.

## 7. API Surface (MVP)
- `POST /auth/register`, `POST /auth/login`
- `GET/POST /workouts`
- `GET/POST /body-metrics`
- `GET /analytics/progress`
- `GET/PUT /goals`

## 8. Delivery Plan (8 Weeks)
1. Week 1: finalize requirements, schema, API contracts.
2. Week 2: implement auth + base CRUD in local Compose.
3. Week 3: add chart-ready analytics endpoints.
4. Week 4: add WebSocket events + reminder worker.
5. Week 5: deploy to Minikube then DOKS with PVC.
6. Week 6: monitoring, alerts, and load/stability checks.
7. Week 7: CI/CD + backup/restore workflow.
8. Week 8: polish UX, testing, demo rehearsal.

## 9. Risks and Mitigations
- Chart complexity risk: start with weekly aggregates first.
- Reminder reliability risk: use idempotent worker jobs and retry policy.
- Scope risk: keep MVP to workout/body metrics before advanced analytics.
