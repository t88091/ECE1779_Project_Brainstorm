# Design Doc: Personal Budget + Bill Reminder + Friend Bill Splitting

## 1. Project Summary
Build a personal finance app for expense tracking, recurring bill reminders, and shared expense splitting with friends.

## 2. Users and Core Use Cases
- User tracks income/expenses by category.
- User configures recurring bills and due dates.
- User creates friend groups for shared expenses.
- User splits bills equally or by custom percentage/amount.
- User marks settlements and tracks who still owes money.

## 3. Proposed Architecture
- `frontend`: Next.js UI for budgets, bill calendar, and group splits.
- `api`: Node.js/Express REST API with transaction-safe write flows.
- `worker`: Scheduled reminders and settlement nudges.
- `functions` (optional): DigitalOcean Functions for event-driven reminders.
- `postgres`: PostgreSQL for financial records and split states.

## 4. Data Model (Initial)
- `users`
- `wallet_accounts`
- `transactions`
- `budget_limits`
- `recurring_bills`
- `friend_groups`
- `shared_expenses`
- `expense_shares`
- `settlements`
- `notification_events`

## 5. Course Requirements Plan

| Requirement | Implementation Plan | Evidence/Demo |
|---|---|---|
| Docker containerization | Dockerfiles for frontend, api, worker. | Container build/run logs. |
| Docker Compose local setup | Compose stack with postgres + api + worker + frontend. | Local end-to-end split flow in one command. |
| PostgreSQL persistence | All transaction, bill, and share state stored in Postgres. | Restart and verify balances are intact. |
| Persistent storage | Postgres on DigitalOcean Block Storage (PVC). | Pod restart without data loss. |
| Deploy to DO/Fly | Deploy to DigitalOcean Kubernetes. | Live URL and Kubernetes objects. |
| Orchestration (K8s/Swarm) | Kubernetes Deployments/Services + PVC + Ingress. | Scale API replicas and verify continuity. |
| Monitoring/observability | DO metrics, API latency/error metrics, reminder job status dashboards. | Alert fired on failed reminder job. |
| At least 2 advanced features | Serverless reminders, security/auth, CI/CD, backup/recovery. | Pipeline + reminder + restore demo. |

## 6. Advanced Features (Selected)
- Serverless integration: reminder dispatch via DigitalOcean Functions.
- Security enhancements: JWT auth, role checks, secrets in Kubernetes Secret.
- CI/CD pipeline: GitHub Actions with automated deployment.
- Backup and recovery: scheduled DB dump and restore procedure.

## 7. API Surface (MVP)
- `POST /auth/register`, `POST /auth/login`
- `GET/POST /transactions`
- `GET/POST /recurring-bills`
- `GET/POST /groups`
- `POST /shared-expenses`
- `POST /settlements`
- `GET /balances/group/:groupId`

## 8. Delivery Plan (8 Weeks)
1. Week 1: schema + split logic design and API contracts.
2. Week 2: auth and expense CRUD in Compose environment.
3. Week 3: recurring bill scheduler and reminder primitives.
4. Week 4: friend groups, shared expense splitting, settlements.
5. Week 5: Kubernetes deployment (Minikube then DOKS) with PVC.
6. Week 6: monitoring dashboards and alerting.
7. Week 7: CI/CD + backup/restore validation.
8. Week 8: edge-case testing and demo hardening.

## 9. Risks and Mitigations
- Split edge-case complexity: constrain MVP to equal/custom split only.
- Data correctness risk: use DB transactions and integrity constraints.
- Notification reliability risk: add retry + dead-letter handling.
