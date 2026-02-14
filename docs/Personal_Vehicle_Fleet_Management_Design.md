# Design Doc: Personal Vehicle Fleet & Car Life Management Platform

## 1. Project Summary

Build a cloud-native personal vehicle management platform designed for
individuals or families to track multiple vehicles, monitor driving
history, log maintenance records, receive service reminders, and view
real-time vehicle status updates.

------------------------------------------------------------------------

## 2. Users and Core Use Cases

Primary Users: - Individual car owners - Family members sharing vehicles

Core Use Cases: 
- User registers and manages their personal vehicle account. 
- User adds one or more vehicles (VIN, model, year, plate number). 
- User logs trips manually (distance, purpose, fuel usage). 
- User records maintenance activities (oil change, tire rotation, repairs). 
- System tracks total mileage and reminds user of upcoming service intervals. 
- User receives alerts for insurance renewal or registration expiry. 
- User views vehicle expense summaries (fuel, maintenance, repairs). 
- Real-time vehicle status simulation (optional feature for demo).

------------------------------------------------------------------------

## 3. Proposed Architecture

-   `frontend`: Simple personal dashboard (Next.js or React) showing
    vehicles, trip logs, expenses, and reminders.
-   `api`: Node.js/Express REST API handling user accounts and vehicle
    operations.
-   `websocket`: Real-time notifications for reminders and status
    updates.
-   `worker`: Background scheduler for maintenance reminders and renewal
    alerts.
-   `postgres`: Persistent storage for users, vehicles, trips, and
    expense data.
-   `ingress`: Exposes application endpoints via DigitalOcean
    Kubernetes.

Deployment Target: - DigitalOcean Kubernetes (DOKS) - PostgreSQL with
PersistentVolumeClaim backed by DigitalOcean Volume - 2 API replicas for
availability - Optional Horizontal Pod Autoscaler for demonstration

------------------------------------------------------------------------

## 4. Data Model (Initial)

-   `users`
-   `vehicles`
-   `trips`
-   `maintenance_records`
-   `service_reminders`
-   `expenses`
-   `insurance_records`
-   `registration_records`
-   `notification_logs`
-   `backup_logs`

------------------------------------------------------------------------

## 5. Course Requirements Plan

  -------------------------------------------------------------------------------
  Requirement                Implementation Plan           Evidence/Demo
  -------------------------- ----------------------------- ----------------------
  Docker containerization    Containerize api, worker, and Multi-service Docker
                             optional frontend.            images.

  Docker Compose local setup Compose stack with postgres + Local vehicle tracking
                             api + worker (+ frontend).    demo.

  PostgreSQL persistence     Store vehicles, trips, and    Data survives
                             expenses in Postgres.         container restart.

  Persistent storage         Postgres PVC backed by        Redeploy without
                             DigitalOcean Volume.          losing data.

  Deploy to DO/Fly           Deploy on DigitalOcean        Public URL and cluster
                             Kubernetes.                   resources.

  Orchestration              Kubernetes Deployments,       Multiple replicas
                             Services, PVC, Ingress.       shown in cluster.

  Monitoring/observability   Monitor CPU, memory, DB disk  Dashboard + alert
                             usage, worker job failures.   example.

  At least 2 advanced        Real-time notifications,      Live reminder demo +
  features                   security/JWT, CI/CD, backup & pipeline demo.
                             recovery.                     
  -------------------------------------------------------------------------------

------------------------------------------------------------------------

## 6. Advanced Features (Selected)

-   Real-time functionality: WebSocket notifications for service
    reminders and renewal alerts.
-   Security enhancements: JWT authentication and user-level data
    isolation.
-   CI/CD: GitHub Actions automated build and deployment pipeline.
-   Backup & recovery: Scheduled PostgreSQL backups stored in cloud
    object storage.
-   Auto-scaling (optional stretch goal): Horizontal Pod Autoscaler
    based on CPU metrics.

------------------------------------------------------------------------

## 7. API Surface (MVP)

### Authentication

-   `POST /auth/register`
-   `POST /auth/login`

### Vehicle Management

-   `POST /vehicles`
-   `GET /vehicles`
-   `PUT /vehicles/:id`
-   `DELETE /vehicles/:id`

### Trip Logging

-   `POST /trips`
-   `GET /trips`

### Maintenance

-   `POST /maintenance`
-   `GET /maintenance`

### Expenses

-   `POST /expenses`
-   `GET /expenses`

### Insurance & Registration

-   `POST /insurance`
-   `GET /insurance`
-   `POST /registration`
-   `GET /registration`

### WebSocket Events

-   `reminderNotification`
-   `vehicleStatusUpdate`

------------------------------------------------------------------------

## 8. Delivery Plan (8 Weeks)

1.  Week 1: Define personal vehicle domain model and schema.
2.  Week 2: Implement authentication and vehicle CRUD using Docker
    Compose.
3.  Week 3: Add trip logging and expense tracking.
4.  Week 4: Implement maintenance reminders and renewal alerts (worker
    service).
5.  Week 5: Deploy to Minikube and DigitalOcean Kubernetes with PVC.
6.  Week 6: Add monitoring dashboards and system alerts.
7.  Week 7: Implement CI/CD pipeline and backup job.
8.  Week 8: Load testing, scaling demonstration, and demo polish.

------------------------------------------------------------------------

## 9. Risks and Mitigations

-   Reminder logic complexity: Start with simple mileage/time-based
    thresholds.
-   Over-expanding features: Limit MVP to 1--3 vehicles per user.
-   Real-time bugs: Emit notification only after successful DB commit.
-   Backup failures: Log backup status and provide manual recovery
    instructions.
