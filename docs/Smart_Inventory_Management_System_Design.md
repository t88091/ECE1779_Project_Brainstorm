# Design Doc: Smart Inventory Management System

## 1. Project Summary

Build a cloud-native inventory management system where businesses manage
stock across multiple locations, track stock movements in real time, and
receive alerts when inventory levels fall below thresholds.

------------------------------------------------------------------------

## 2. Users and Core Use Cases

-   User registers and logs into the system.
-   Admin creates and manages user roles (Admin, Manager, Staff).
-   Manager creates items (SKU, category, unit, low-stock threshold).
-   Manager creates warehouse/locations.
-   Manager or Staff performs stock operations (receive, ship, transfer,
    adjust).
-   Users view real-time inventory updates.
-   System triggers alerts when stock drops below threshold.
-   Users review transaction history and audit logs.

------------------------------------------------------------------------

## 3. Proposed Architecture

-   `frontend`: Optional Next.js dashboard for inventory overview and
    stock operations.
-   `api`: Node.js/Express REST API + WebSocket server for real-time
    updates.
-   `worker`: Background jobs for low-stock detection and backup
    scheduling.
-   `postgres`: Persistent inventory, user, and transaction data.
-   `ingress`: Exposes public API and frontend endpoints on DigitalOcean
    Kubernetes.

Deployment Target: - DigitalOcean Kubernetes (DOKS) - PostgreSQL with
PersistentVolumeClaim backed by DigitalOcean Volume - Multiple API
replicas with load balancing

------------------------------------------------------------------------

## 4. Data Model (Initial)

-   `users`
-   `roles`
-   `items`
-   `locations`
-   `inventory`
-   `transactions`
-   `alerts`
-   `backup_logs`

------------------------------------------------------------------------

## 5. Course Requirements Plan

  -------------------------------------------------------------------------------
  Requirement                Implementation Plan           Evidence/Demo
  -------------------------- ----------------------------- ----------------------
  Docker containerization    Containerize api, worker, and Multi-container image
                             optional frontend.            build proof.

  Docker Compose local setup Compose stack with postgres + Local real-time stock
                             api + worker (+ frontend).    update demo.

  PostgreSQL persistence     Inventory state and           Data survives
                             transactions stored in        container restart.
                             Postgres.                     

  Persistent storage         Postgres PVC backed by        Redeploy without
                             DigitalOcean Volume.          losing inventory data.

  Deploy to DO/Fly           Use DigitalOcean Kubernetes.  Public cluster
                                                           deployment.

  Orchestration (K8s/Swarm)  Kubernetes Deployments,       Replicas scale under
                             Services, PVC, Ingress.       load.

  Monitoring/observability   Track API CPU/memory, DB disk Dashboard
                             usage, worker job failures.   screenshots + alert
                                                           demo.

  At least 2 advanced        Real-time updates,            Live stock update +
  features                   security/RBAC, CI/CD, backup  pipeline demo.
                             & recovery.                   
  -------------------------------------------------------------------------------

------------------------------------------------------------------------

## 6. Advanced Features (Selected)

-   Real-time functionality: WebSocket-based live inventory updates.
-   Security enhancements: JWT authentication and role-based access
    control.
-   CI/CD: GitHub Actions pipeline for automated build and deployment.
-   Backup & recovery: Scheduled PostgreSQL backups stored in cloud
    object storage.
-   Auto-scaling (optional stretch goal): Horizontal Pod Autoscaler
    based on CPU.

------------------------------------------------------------------------

## 7. API Surface (MVP)

### Authentication

-   `POST /auth/register`
-   `POST /auth/login`

### Item Management

-   `POST /items`
-   `GET /items`
-   `PUT /items/:id`
-   `DELETE /items/:id`

### Location Management

-   `POST /locations`
-   `GET /locations`

### Inventory Operations

-   `POST /inventory/receive`
-   `POST /inventory/ship`
-   `POST /inventory/transfer`
-   `GET /inventory`

### Transactions & Alerts

-   `GET /transactions`
-   `GET /alerts`

### WebSocket Events

-   `stockUpdated`
-   `lowStockAlert`

------------------------------------------------------------------------

## 8. Delivery Plan (8 Weeks)

1.  Week 1: Define domain model, schema design, role system.
2.  Week 2: Authentication + item/location CRUD in Docker Compose.
3.  Week 3: Inventory operations + transaction logging.
4.  Week 4: Implement WebSocket real-time updates.
5.  Week 5: Deploy to Minikube and DigitalOcean Kubernetes with PVC.
6.  Week 6: Implement monitoring dashboards and alert configuration.
7.  Week 7: Add CI/CD pipeline and automated backup job.
8.  Week 8: Load testing, auto-scaling validation, demo polish.

------------------------------------------------------------------------

## 9. Risks and Mitigations

-   Concurrent stock updates: use database transactions with row-level
    locking.
-   Scope creep: limit MVP to single-organization inventory management.
-   Backup complexity: start with simple pg_dump cron job before
    advanced recovery.
-   Real-time sync bugs: ensure event emission occurs only after DB
    commit.
