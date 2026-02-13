## Project Idea[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#project-idea)

The course project represents a more innovative and time-consuming piece of work than the course assignments, requiring teamwork to build a **stateful cloud-native application** (e.g., a web app or service that maintains user data across sessions/restarts). It consists of four key stages: **team formation**, **project proposal**, **in-class presentation**, and **final deliverable submission by the specified deadline**.

Your project MUST be deployed to a cloud or edge provider and MUST incorporate the following technologies and features:

### Core Technical Requirements[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#core-technical-requirements)

#### Containerization and Local Development (Required for ALL projects)[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#containerization-and-local-development-required-for-all-projects)

- Use **Docker** to containerize the application (e.g., Node.js backend, database).
- Use **Docker Compose** for multi-container setup (e.g., app + database).

#### State Management (Required for ALL projects)[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#state-management-required-for-all-projects)

- Use **PostgreSQL** for relational data persistence.
- Implement **persistent storage** (e.g., [DigitalOcean Volumes](https://docs.digitalocean.com/products/volumes/)  or [Fly Volumes](https://fly.io/docs/volumes/) ) to ensure state survives container restarts or redeployments.

#### Deployment Provider (Required for ALL projects)[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#deployment-provider-required-for-all-projects)

- Deploy to either **DigitalOcean** (IaaS focus) or **Fly.io** (edge/PaaS focus).

#### Orchestration Approach (Choose ONE)[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#orchestration-approach-choose-one)

**Option A: Docker Swarm Mode**

- Use Docker Swarm mode for clustering and orchestration.
- Implement service replication and load balancing.

**Option B: Kubernetes**

- Use Kubernetes for orchestration (start with minikube locally, then deploy to a cloud-managed cluster, e.g., [DigitalOcean Kubernetes](https://www.digitalocean.com/products/kubernetes) ).
- Include Deployments, Services, and PersistentVolume for stateful data.

**Note on Orchestration on Fly.io**

Using Fly.io’s built-in scaling, replication, or global distribution features alone does **not** satisfy the orchestration requirement, because these are provider-level capabilities rather than an implementation of Docker Swarm or Kubernetes.

[Fly Kubernetes Service (FKS)](https://fly.io/docs/kubernetes/fks-quickstart/)  is **not** required. It is a paid beta feature ($75/month per cluster) and may have limited documentation or bugs. You may use it if you wish, but it is not recommended.

For Kubernetes, please use minikube locally plus a managed Kubernetes service — DigitalOcean Kubernetes — or Docker Swarm on Fly.io/DigitalOcean. These options fully satisfy the course requirements and avoid unnecessary cost or complexity.

#### Monitoring and Observability (Required for ALL projects)[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#monitoring-and-observability-required-for-all-projects)

- Integrate monitoring using provider tools (e.g., DigitalOcean metrics/alerts for CPU, memory, disk; Fly.io logs/metrics).
- Set up basic alerts or dashboards for key metrics.
- *(Optional)* You may also integrate metrics/logs/traces into your frontend if you have one to make the demo clearer.

**Note on Frontend (UI)**

A frontend is **not required**, but you may include a simple web interface (e.g., built with [Next.js](https://nextjs.org/)  or another framework) to make your demo and presentation clearer. The grading focus is on your backend, architecture, and deployment, not UI complexity.

### Advanced Features (Must implement at least two)[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#advanced-features-must-implement-at-least-two)

- Serverless integration (e.g., DigitalOcean Functions or Fly.io for event-driven tasks like notifications).
- Real-time functionality (e.g., WebSockets for live updates).
- Auto-scaling and high availability (e.g., configure Swarm/K8s to scale based on load).
- Security enhancements (e.g., authentication/authorization, HTTPS, secrets management).
- CI/CD pipeline (e.g., GitHub Actions for automated builds/deployments).
- Backup and recovery (e.g., automated database backups to cloud storage).
- Integration with external services (e.g., email notifications via SendGrid).
- Edge-specific optimizations (e.g., global distribution on Fly.io with region-based routing).

### Note on Project Requirements and Scope[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#note-on-project-requirements-and-scope)

**Meeting the [Core Technical Requirements](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#core-technical-requirements) is mandatory — no exceptions.** Projects must use specified tools (e.g., DigitalOcean/Fly.io, PostgreSQL) to align with course content, ensure fair grading and peer review, and keep costs/complexity manageable. Alternative platforms (e.g., AWS/GCP) are not permitted. However, students are encouraged to explore advanced features with any modern tools as long as they are compatible with the core technical requirements.

Your project MUST implement all core technologies and at least two advanced features. To ensure an achievable project within the ~2-month timeline, avoid these pitfalls:

- **Too simple**: A basic CRUD app without orchestration or monitoring shows insufficient mastery.
- **Too complex**: Distributed systems or ML integrations are hard to complete effectively.
- **Too broad**: A full-scale platform (e.g., a cloud-native social media app) risks shallow implementation.

The **goal** is to develop a **well-scoped, thoughtfully implemented application** that effectively showcases your understanding of cloud computing principles using the required technologies. A successful project should have a **clear purpose, achievable scope, and polished implementation of core features**.

Remember: A well-executed, focused project is far more valuable than an overly ambitious one that cannot be completed properly within the course timeline. While creativity and innovation are encouraged, please keep in mind that this is a course project with specific learning objectives to achieve.

### Example Project Ideas[](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#example-project-ideas)

Below are some inspiring project ideas to guide your team. Smaller teams (2 members) may implement a subset of features, while larger teams (3-4 members) should aim for more features or added complexity. **All projects must fulfill the [Core Technical Requirements](https://www.eecg.utoronto.ca/~cying/courses/ece1779-cloud/project/handout/#core-technical-requirements)**. The listed features illustrate potential scope rather than strict expectations. Teams are also encouraged to propose unique project ideas tailored to their interests and expertise, provided the scope is realistic and achievable within the project timeline.

1. **Collaborative Task Management Platform**
   
   A cloud-native web application for teams to manage tasks with real-time updates and persistent data storage.
   
   **Key Features**:
   
   - User authentication and team-based project management
   - Role-based access control (e.g., Admin, Member)
   - Task creation, assignment, and status tracking (e.g., To-Do, In Progress, Done)
   - Real-time task updates using WebSockets
   - Persistent storage for tasks and user data using PostgreSQL and DigitalOcean Volumes
   - Dockerized Node.js backend with Docker Compose for local development
   - Deployment on DigitalOcean with Docker Swarm for orchestration
   - Monitoring dashboard for CPU, memory, and task activity metrics
   - Automated database backups to cloud storage
   - CI/CD pipeline with GitHub Actions for automated deployments
   - Search functionality for tasks by keyword or assignee

---

2. **Event Logging and Analytics System**
   
   A platform to collect, store, and analyze event data (e.g., IoT sensor readings or user interactions) with real-time insights.
   
   **Key Features**:
   
   - User authentication and organization-based access
   - REST API for event ingestion and data retrieval
   - Real-time event visualization with dynamic charts
   - PostgreSQL for event storage with Fly.io persistent volumes
   - Dockerized Python backend with Docker Compose for local setup
   - Deployment on Fly.io with Kubernetes for orchestration
   - Monitoring alerts for high event volume or system health
   - Automated backups of event data to cloud storage
   - Auto-scaling based on event ingestion load
   - Search and filter events by timestamp, type, or source
   - Serverless notifications for critical events (e.g., via DigitalOcean Functions)

---

3. **Content Sharing and Collaboration Platform**
   
   A cloud-based system for users to upload, share, and collaborate on content (e.g., documents, images).
   
   **Key Features**:
   
   - User authentication with role-based permissions (e.g., Owner, Collaborator, Viewer)
   - File upload and management with version history
   - Collaborative commenting and tagging on content
   - PostgreSQL for metadata and user data with DigitalOcean Volumes
   - Dockerized Node.js backend with Docker Compose for local testing
   - Deployment on DigitalOcean with Docker Swarm for load balancing
   - Monitoring for storage usage and API performance metrics
   - CI/CD pipeline using GitHub Actions for automated builds
   - Search functionality for content by tags or metadata
   - HTTPS and authentication for secure access
   - Export content metadata in JSON or CSV formats

---

4. **Inventory Management System**
   
   A cloud-native application for tracking and managing inventory with real-time updates and edge optimizations.
   
   **Key Features**:
   
   - User authentication with role-based access (e.g., Manager, Staff)
   - Inventory CRUD operations (create, read, update, delete) for items
   - Real-time stock updates using WebSockets
   - PostgreSQL for inventory data with Fly.io persistent volumes
   - Dockerized Python backend with Docker Compose for local development
   - Deployment on Fly.io with Kubernetes for orchestration
   - Monitoring dashboard for inventory levels and system health
   - Automated backups of inventory data to cloud storage
   - Edge routing for low-latency access across regions
   - Search and filter inventory by category, quantity, or location
   - Serverless email notifications for low-stock alerts

---

5. **Scientific Data Repository**
   
   A platform for researchers to store, share, and analyze scientific datasets with access control and visualization.
   
   **Key Features**:
   
   - User authentication and role-based permissions (e.g., Researcher, Admin)
   - Dataset upload and metadata management (e.g., tags, descriptions)
   - Access control for public/private datasets
   - PostgreSQL for dataset metadata with DigitalOcean Volumes
   - Dockerized Python backend with Docker Compose for local setup
   - Deployment on DigitalOcean with Docker Swarm for orchestration
   - Monitoring for storage usage and dataset access metrics
   - Visualization dashboard for dataset trends
   - Serverless notifications for dataset updates
   - Search functionality for datasets by metadata or tags
   - Backup and recovery system for dataset integrity
