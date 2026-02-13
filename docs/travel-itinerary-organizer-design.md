# Design Doc: Travel Itinerary Organizer

## 1. Project Summary
Build an itinerary management app where users organize trip plans, collaborate with travel companions, and track trip checklists.

## 2. Users and Core Use Cases
- User creates trips with dates and destinations.
- User adds itinerary items (flight, hotel, transport, activities, notes).
- User invites collaborators with view/edit permissions.
- Users track completion checklists (passport, visa, packing).
- Users receive reminders for upcoming itinerary events.

## 3. Proposed Architecture
- `frontend`: Next.js trip timeline and checklist UI.
- `api`: Node.js/Express REST API + WebSocket for collaborative updates.
- `worker`: Reminder jobs and optional external status sync.
- `postgres`: Persistent trip data and collaboration metadata.
- `ingress`: Exposes app endpoints on DOKS.

## 4. Data Model (Initial)
- `users`
- `trips`
- `trip_members`
- `itinerary_items`
- `checklist_items`
- `attachments`
- `notification_events`
- `external_sync_logs`

## 5. Course Requirements Plan

| Requirement | Implementation Plan | Evidence/Demo |
|---|---|---|
| Docker containerization | Containerize frontend, api, worker. | Multi-service image build proof. |
| Docker Compose local setup | Compose stack with postgres + api + worker + frontend. | Local collaborative editing demo. |
| PostgreSQL persistence | Trip state, members, and timeline data in Postgres. | Data survives pod/container restart. |
| Persistent storage | Postgres PVC backed by DigitalOcean Volume. | Redeploy without trip data loss. |
| Deploy to DO/Fly | Use DigitalOcean Kubernetes. | Public deployment and cluster resources. |
| Orchestration (K8s/Swarm) | Kubernetes Deployments, Services, PVC, Ingress. | Replicas increase under load test. |
| Monitoring/observability | Track API errors, WebSocket connection counts, worker job failures. | Dashboard and alert examples. |
| At least 2 advanced features | Real-time collaboration, security/RBAC, CI/CD, optional external API sync. | Live collaboration and pipeline demo. |

## 6. Advanced Features (Selected)
- Real-time functionality: live itinerary update events over WebSockets.
- Security enhancements: role-based permissions for trip collaborators.
- CI/CD: automated tests/build/deploy pipeline.
- External integration (optional): weather or flight status API sync.

## 7. API Surface (MVP)
- `POST /auth/register`, `POST /auth/login`
- `POST /trips`, `GET /trips/:id`
- `POST /trips/:id/members`
- `GET/POST/PUT/DELETE /trips/:id/items`
- `GET/POST/PUT /trips/:id/checklist`

## 8. Delivery Plan (8 Weeks)
1. Week 1: domain model and collaboration rules.
2. Week 2: auth + trip CRUD in Compose.
3. Week 3: itinerary timeline endpoints and checklist flow.
4. Week 4: WebSocket collaboration and permission checks.
5. Week 5: Kubernetes deployment to Minikube and DOKS with PVC.
6. Week 6: reminder worker and observability dashboards.
7. Week 7: CI/CD pipeline and integration hardening.
8. Week 8: conflict-handling tests and demo polish.

## 9. Risks and Mitigations
- Concurrent edit conflicts: use last-write-wins plus change timestamps for MVP.
- External API instability: keep sync optional and cache data.
- Scope growth risk: skip file uploads in MVP if needed.
