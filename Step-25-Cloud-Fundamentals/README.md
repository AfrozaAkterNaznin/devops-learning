# Step 25 — Cloud Fundamentals

## Part 1 — Cloud Computing Fundamentals

---

## 1. Overview

Cloud Computing is the use of computing resources over a network instead of depending entirely on locally owned physical infrastructure.

Common cloud resources include:

- Compute
- Storage
- Networking
- Databases
- Security
- Monitoring
- Application hosting

The main idea is:

    Local Infrastructure
        ↓
    Own / manage hardware

    Cloud Infrastructure
        ↓
    Use infrastructure provided by a cloud provider

Cloud does not simply mean "someone else's computer".

It is a complete model for accessing computing infrastructure and services on demand.

---

## 2. Local Infrastructure vs Cloud

Before learning Cloud, the difference between traditional local infrastructure and cloud infrastructure needs to be clear.

### Local / Traditional Model

    Physical Server
        ↓
    CPU + RAM + Storage
        ↓
    Operating System
        ↓
    Application
        ↓
    Users

The organization is responsible for the physical infrastructure and its maintenance.

### Cloud Model

    User
      ↓
    Internet
      ↓
    Cloud Provider
      ↓
    Compute / Network / Storage / Database
      ↓
    Application

The cloud provider manages the underlying physical infrastructure while the customer uses the required services.

---

## 3. Local vs Cloud Comparison

| Feature | Local / On-Premises | Cloud |
|---|---|---|
| Hardware | Owned or directly managed | Provided by cloud provider |
| Initial infrastructure cost | Usually high | Usually lower initial cost |
| Hardware maintenance | Customer | Mostly provider |
| Scaling | Often slower | Easier to scale |
| Resource provisioning | Manual / physical | Usually faster |
| Geographic options | Limited by owned infrastructure | Multiple regions available |
| Infrastructure flexibility | Limited by hardware | High |
| Operational responsibility | Higher | Shared with provider |

---

## 4. Why Cloud Is Important in DevOps

DevOps is not only about writing automation.

A production application needs an environment where it can run.

The progression learned so far is:

    Application
        ↓
    Linux
        ↓
    Networking
        ↓
    Nginx
        ↓
    Docker
        ↓
    Docker Compose
        ↓
    GitHub Actions
        ↓
    Cloud Infrastructure

Cloud provides the infrastructure layer where applications can eventually run at scale.

---

## 5. Cloud Computing

Cloud Computing means accessing computing resources through a network, usually the Internet, without having to own and maintain all of the underlying physical infrastructure.

Typical resources include:

    Compute
    Storage
    Network
    Database
    Security
    Application platforms

The user consumes the required resources while the cloud provider operates the underlying physical infrastructure.

---

## 6. Main Characteristics of Cloud

### 6.1 On-Demand Resources

Resources can be provisioned when needed.

Conceptually:

    Need resource
        ↓
    Provision resource
        ↓
    Use resource

This is different from purchasing physical hardware before the resource is required.

---

### 6.2 Scalability

Scalability means increasing or decreasing system capacity according to requirements.

Example:

    Low traffic
        ↓
    Fewer resources

    High traffic
        ↓
    More resources

Cloud platforms make this process easier than manually purchasing and installing additional physical servers.

---

### 6.3 Elasticity

Elasticity means adjusting resources according to changing workload or demand.

Conceptually:

    Traffic increases
        ↓
    Capacity increases

    Traffic decreases
        ↓
    Capacity decreases

Scalability and elasticity are related, but elasticity emphasizes dynamic adjustment according to changing demand.

---

### 6.4 Availability

Availability describes how consistently a service remains accessible and operational.

A production system should minimize unnecessary downtime.

Cloud infrastructure provides features that can be used to design highly available systems.

Availability becomes especially important when applications serve real users continuously.

---

### 6.5 Pay-as-You-Go

Many cloud services use usage-based pricing models.

Conceptually:

    Use resources
        ↓
    Pay according to usage / selected pricing model

This can reduce the need for large upfront hardware investment.

However, cloud resources can still generate significant costs if they are left running or incorrectly configured.

Therefore:

    Cloud ≠ Free

Cost awareness is an important part of cloud engineering.

---

## 7. Cloud Provider

Cloud Computing is the concept.

A Cloud Provider is the company that provides the cloud infrastructure and services.

Common providers include:

| Provider | Full Name |
|---|---|
| AWS | Amazon Web Services |
| Azure | Microsoft Azure |
| GCP | Google Cloud Platform |

The concepts learned in this step are mostly provider-independent.

Specific provider services can implement those concepts differently.

---

## 8. Cloud Service Models

Cloud services are commonly categorized into:

    IaaS
    PaaS
    SaaS

These models describe how much infrastructure and platform responsibility is handled by the provider.

---

## 9. IaaS

IaaS = Infrastructure as a Service

IaaS provides fundamental infrastructure resources such as:

- Compute
- Virtual machines
- Storage
- Networking

The customer has relatively high control over the environment.

Conceptually:

    Cloud Provider
        ↓
    Hardware
        ↓
    Virtualization
        ↓
    Infrastructure
        ↓
    Customer manages:
        OS
        Runtime
        Application
        Configuration

IaaS gives more control but also more management responsibility.

---

## 10. PaaS

PaaS = Platform as a Service

A PaaS provider manages more of the underlying infrastructure and runtime environment.

The developer can focus more on the application.

Conceptually:

    Cloud Provider
        ↓
    Infrastructure
        ↓
    Operating System
        ↓
    Runtime / Platform
        ↓
    Customer
        ↓
    Application

PaaS generally provides less infrastructure control than IaaS but reduces operational management.

---

## 11. SaaS

SaaS = Software as a Service

SaaS provides a ready-to-use software application.

The user generally does not manage the underlying infrastructure, operating system, or application runtime.

Conceptually:

    Provider
        ↓
    Infrastructure
        ↓
    Platform
        ↓
    Application
        ↓
    User

Examples include web-based productivity and communication applications.

The user primarily consumes the software instead of managing the infrastructure behind it.

---

## 12. IaaS vs PaaS vs SaaS

| Feature | IaaS | PaaS | SaaS |
|---|---|---|---|
| Control | High | Medium | Low |
| Infrastructure management | More customer responsibility | Mostly provider | Provider |
| OS management | Customer | Usually provider | Provider |
| Runtime management | Customer | Mostly provider | Provider |
| Application management | Customer | Customer | Provider |
| User responsibility | Highest | Medium | Lowest |
| Main focus | Infrastructure | Application development | Software usage |

General relationship:

    More control
        ↓
    IaaS
        ↓
    PaaS
        ↓
    SaaS
        ↓
    Less infrastructure responsibility

---

## 13. Responsibility Difference

The three service models can be understood through responsibility.

### IaaS

    Provider
        → Physical infrastructure

    Customer
        → OS
        → Runtime
        → Application
        → Data
        → Configuration

### PaaS

    Provider
        → Infrastructure
        → OS
        → Runtime / Platform

    Customer
        → Application
        → Data

### SaaS

    Provider
        → Infrastructure
        → Platform
        → Application

    Customer
        → Mainly software usage and account/data-related responsibilities

These are simplified conceptual models; exact responsibility boundaries depend on the service.

---

## 14. DevOps Perspective

From a DevOps perspective, the important question is not:

    "Which cloud service name should I memorize?"

The important question is:

    "Which infrastructure does my application require?"

For example, a web application may require:

    Internet access
        ↓
    Frontend hosting
        ↓
    Backend compute
        ↓
    Database
        ↓
    Persistent storage
        ↓
    Security controls

Cloud services provide different ways to implement these requirements.

---

## 15. Our MERN Project and Cloud

The MERN project used throughout the previous Docker and Docker Compose steps contains:

    Client
    Server
    MongoDB

Locally, the application runs using containers.

Conceptually:

    React Client
        ↓
    Node / Express Server
        ↓
    MongoDB

Cloud provides the infrastructure layer where these application components can eventually be hosted.

The exact cloud implementation will be studied separately.

---

## 16. Local Docker vs Cloud

Docker and Cloud solve different problems.

| Technology | Main Purpose |
|---|---|
| Docker | Package and run applications in containers |
| Docker Compose | Run multiple related containers together |
| Cloud | Provide infrastructure and managed services |
| GitHub Actions | Automate CI/CD workflows |

Therefore:

    Docker ≠ Cloud

and:

    Cloud ≠ Docker

They can work together.

Example:

    Cloud Compute
        ↓
    Docker
        ↓
    Application Container

---

## 17. Development Progression

The learning progression up to this point is:

    Local Application
        ↓
    Linux Environment
        ↓
    Docker
        ↓
    Docker Compose
        ↓
    GitHub Actions
        ↓
    Cloud Fundamentals

This creates the foundation for later infrastructure automation and orchestration.

---

## 18. Important Terminology

| Term | Meaning |
|---|---|
| Cloud Computing | Using computing resources through a network |
| Cloud Provider | Company providing cloud infrastructure/services |
| Compute | Resources used to run processes/applications |
| Storage | Resources used to store data |
| Network | Communication infrastructure |
| Region | Geographic cloud location |
| Availability | Ability of a service to remain accessible |
| Scalability | Ability to increase/decrease capacity |
| Elasticity | Dynamic adjustment according to demand |
| IaaS | Infrastructure as a Service |
| PaaS | Platform as a Service |
| SaaS | Software as a Service |

Region and Availability Zone will be explored more deeply in Part 2.

---

## 19. Key Takeaways

### Cloud

    Cloud
      =
    On-demand access to computing infrastructure/services

### IaaS

    More control
        +
    More responsibility

### PaaS

    Less infrastructure management
        +
    More focus on application

### SaaS

    Ready-to-use software
        +
    Minimal infrastructure management

---

## 20. Part 1 Summary

Cloud Computing provides access to computing resources without requiring the customer to own and maintain all underlying physical infrastructure.

The three major service models are:

    IaaS
      ↓
    Infrastructure

    PaaS
      ↓
    Platform

    SaaS
      ↓
    Software

The major DevOps connection is:

    Application
        ↓
    Container
        ↓
    CI/CD
        ↓
    Cloud Infrastructure

This understanding provides the foundation for the infrastructure and architecture topics covered in the next parts.

# Step 25 — Cloud Fundamentals

## Part 2 — Cloud Infrastructure

---

## 1. Overview

Cloud infrastructure is the collection of computing, networking, storage, and database resources required to run applications in the cloud.

The main infrastructure components studied in this part are:

- Region
- Availability Zone
- Compute
- Network
- Storage
- Database
- Load Balancer

These components work together to create an environment where an application can run reliably.

---

## 2. Cloud Infrastructure Overview

A simplified cloud infrastructure can be represented as:

    Cloud Provider
        │
        └── Region
              │
              ├── Availability Zone
              │       │
              │       ├── Compute
              │       ├── Network
              │       └── Storage
              │
              └── Database

Applications use these infrastructure components according to their requirements.

---

## 3. Region

A Region is a geographic area where a cloud provider operates cloud infrastructure.

Conceptually:

    Cloud Provider
        │
        ├── Region A
        ├── Region B
        └── Region C

Choosing a region can affect:

- Network latency
- Data residency
- Availability
- Compliance
- Service availability
- Cost

The application should generally be deployed in a region appropriate for its users and requirements.

---

## 4. Why Region Matters

Suppose most users are located near one geographic area.

A region geographically closer to those users can reduce network latency.

Conceptually:

    User
      │
      ↓
    Nearby Region
      │
      ↓
    Application

Compared with:

    User
      │
      ↓
    Distant Region
      │
      ↓
    Application

Therefore, region selection is an architectural decision rather than simply a location preference.

---

## 5. Availability Zone

An Availability Zone (AZ) is an isolated infrastructure location within a cloud region.

A region can contain multiple Availability Zones.

Conceptually:

    Region
       │
       ├── AZ-1
       ├── AZ-2
       └── AZ-3

Availability Zones help design systems that can tolerate failures affecting one infrastructure location.

---

## 6. Region vs Availability Zone

| Region | Availability Zone |
|---|---|
| Geographic cloud location | Isolated location within a region |
| Larger geographic boundary | Smaller infrastructure boundary |
| Can contain multiple AZs | Belongs to a region |
| Important for location and latency | Important for availability and resilience |

Relationship:

    Region
       │
       ├── AZ-1
       ├── AZ-2
       └── AZ-3

A region and an availability zone are therefore not interchangeable concepts.

---

## 7. High Availability Concept

If an application depends on only one infrastructure location:

    Application
        ↓
      AZ-1

A failure affecting that location may make the application unavailable.

A multi-AZ architecture can reduce this dependency:

    Region
       │
       ├── AZ-1 → Application Instance
       │
       └── AZ-2 → Application Instance

Traffic can be distributed between available instances.

This is one of the reasons cloud architecture uses multiple Availability Zones.

---

## 8. Compute

Compute refers to resources used to execute applications and processes.

Examples include:

- Virtual machines
- Containers
- Managed application runtimes
- Serverless compute

Conceptually:

    Application
        ↓
      Compute
        ↓
    CPU + Memory + Runtime
        ↓
    Process execution

For a MERN application, compute resources can run:

- React/Nginx
- Node.js
- Express
- Docker containers

---

## 9. Compute in Our MERN Project

Our existing project contains:

    Client
        ↓
    React application

    Server
        ↓
    Node.js + Express application

In the Docker-based environment:

    Docker
       │
       ├── Client Container
       └── Server Container

The containers require compute resources to execute.

Therefore:

    Cloud Compute
        ↓
    Docker
        ↓
    MERN Application

Docker packages the application, while compute provides the environment where the application runs.

---

## 10. Network

A cloud network provides communication between application components.

A simplified application network looks like:

    User
      ↓
    Internet
      ↓
    Frontend
      ↓
    Backend
      ↓
    Database

The network determines how these components communicate.

---

## 11. Main Network Responsibilities

Cloud networking can provide:

- Connectivity
- Routing
- Traffic control
- Network isolation
- Internet access
- Internal service communication

A production architecture should separate public-facing components from internal components where appropriate.

---

## 12. Public vs Internal Network Concept

A simplified architecture:

    Internet
       │
       ↓
    Public Entry
       │
       ↓
    Application
       │
       ↓
    Internal Database

The database does not necessarily need direct Internet access.

This reduces unnecessary exposure and creates a more controlled architecture.

Security details are covered separately in Part 3.

---

## 13. Storage

Storage provides persistent space for data.

Applications may need storage for:

- Files
- Images
- Documents
- Backups
- Logs
- Application data

Conceptually:

    Application
        ↓
      Storage
        ↓
    Persistent Data

Storage is important because application processes and containers can be temporary.

---

## 14. Persistent Storage

Persistent storage means data remains available beyond the lifecycle of a temporary process or container.

Without persistent storage:

    Container
        ↓
    Data
        ↓
    Container removed
        ↓
    Data may be lost

With persistent storage:

    Container
        ↓
    Persistent Storage
        ↓
    Container removed
        ↓
    Storage remains
        ↓
    New container
        ↓
    Data available

This concept was observed practically with the MongoDB Docker volume in our project.

---

## 15. Docker Volume and Cloud Storage Concept

Our Docker Compose project uses:

    mongo-data

The MongoDB container mounts persistent data to:

    /data/db

Conceptually:

    MongoDB Container
          │
          ↓
       /data/db
          │
          ↓
      mongo-data
          │
          ↓
    Persistent Storage

This is a local Docker implementation of persistent data storage.

Docker volumes and cloud storage are not identical technologies, but they demonstrate the same important architectural principle:

    Application lifecycle
           ≠
    Persistent data lifecycle

---

## 16. Database

A database stores application data in a persistent and structured form.

Our MERN project uses:

    MongoDB

Application flow:

    React Client
        ↓
    Node / Express
        ↓
    MongoDB

The backend communicates with the database instead of allowing the frontend to directly access the database.

---

## 17. Database in Cloud Architecture

A cloud application can use:

- Self-managed database
- Managed database service

### Self-managed

    Compute
       ↓
    Database software
       ↓
    Database

The engineering team is responsible for more infrastructure management.

### Managed Database

    Application
       ↓
    Managed Database Service

The cloud provider manages more of the underlying infrastructure.

Managed database services can reduce operational workload.

---

## 18. Database and Persistence

A database is not simply another temporary application container.

Database data must persist.

Conceptually:

    Backend
       ↓
    Database
       ↓
    Persistent Storage

For production systems, database durability and backup strategy are important architectural concerns.

---

## 19. Load Balancer

A Load Balancer receives incoming traffic and distributes it across multiple application instances.

Conceptually:

    Internet
        ↓
    Load Balancer
       /   |   \
      ↓    ↓    ↓
    App1 App2 App3

This can help with:

- Traffic distribution
- Availability
- Scaling
- Failure handling

---

## 20. Why Load Balancing Is Useful

With only one application instance:

    User Traffic
         ↓
      Server 1

If the server becomes unavailable, the application may become unavailable.

With multiple instances:

    User Traffic
         ↓
    Load Balancer
       /       \
      ↓         ↓
    Server 1  Server 2

If one instance becomes unavailable, traffic can potentially be directed to another healthy instance.

---

## 21. MERN Application Cloud Mapping

Our existing MERN application can be mapped conceptually to cloud infrastructure.

| MERN Component | Cloud Infrastructure Concept |
|---|---|
| React Client | Frontend hosting / compute |
| Node.js Server | Backend compute |
| Express | Backend application runtime |
| MongoDB | Database |
| Docker | Application containerization |
| Docker Compose | Local multi-container environment |
| Docker Network | Application network |
| `mongo-data` | Persistent storage concept |
| Nginx | Web server / reverse proxy |
| GitHub Actions | CI automation |

This is a conceptual mapping.

The exact cloud service used for each component depends on the cloud provider and architecture.

---

## 22. Current Local Architecture

Our Docker Compose application currently contains:

    Client
       │
       ↓
    Server
       │
       ↓
    MongoDB
       │
       ↓
    mongo-data

The containers communicate through the Docker Compose network.

The project also publishes:

    3000:80
    5001:5001
    27017:27017

These port mappings are part of the current local development environment.

---

## 23. Local Port Mapping

### Client

    3000:80

Meaning:

    Host Port 3000
         ↓
    Container Port 80

The React/Nginx container is therefore accessible through port 3000 on the host.

### Server

    5001:5001

Meaning:

    Host Port 5001
         ↓
    Container Port 5001

The backend is accessible through port 5001 on the host.

### MongoDB

    27017:27017

Meaning:

    Host Port 27017
         ↓
    MongoDB Container Port 27017

This makes MongoDB reachable through the host port in the current local setup.

For production cloud architecture, the database would normally be kept private rather than directly exposed to the public Internet.

---

## 24. Cloud Architecture for the MERN Application

A conceptual production-style architecture can look like:

    Internet
        │
        ↓
    Load Balancer / Public Entry
        │
        ↓
    Frontend
        │
        ↓
    Backend Compute
        │
        ↓
    Private Database
        │
        ↓
    Persistent Storage

The exact implementation depends on the cloud provider and deployment strategy.

---

## 25. Complete Infrastructure Relationship

The major infrastructure concepts can be connected as:

    Cloud Provider
        │
        ↓
      Region
        │
        ↓
    Availability Zones
        │
        ├───────────────┐
        ↓               ↓
     Network          Compute
                         │
                         ↓
                      Docker
                         │
                    ┌────┴────┐
                    ↓         ↓
                 Client     Server
                              │
                              ↓
                          Database
                              │
                              ↓
                    Persistent Storage

A Load Balancer can be placed before application instances:

    Internet
        ↓
    Load Balancer
        ↓
    Application Instances
        ↓
    Database
        ↓
    Persistent Storage

---

## 26. Infrastructure Layers

Cloud infrastructure can be viewed in layers:

| Layer | Main Purpose |
|---|---|
| Region | Geographic placement |
| Availability Zone | Infrastructure isolation |
| Network | Communication and routing |
| Compute | Run applications |
| Storage | Persist files/data |
| Database | Store application data |
| Load Balancer | Distribute traffic |

These layers work together rather than operating independently.

---

## 27. Infrastructure vs Application

It is important to distinguish application components from infrastructure components.

### Application

    React
    Node.js
    Express
    MongoDB application data

### Infrastructure

    Compute
    Network
    Storage
    Region
    Availability Zone
    Load Balancer

Docker connects the application and execution environment:

    Application
        ↓
      Docker
        ↓
    Compute Infrastructure

---

## 28. Key Takeaways

### Region

    Geographic cloud location

### Availability Zone

    Isolated infrastructure location inside a region

### Compute

    Runs application processes

### Network

    Connects application components

### Storage

    Persists files and data

### Database

    Stores application data

### Load Balancer

    Distributes incoming traffic

---

## 29. Part 2 Summary

Cloud infrastructure is made of multiple components that work together.

The main concepts are:

    Region
       ↓
    Availability Zone
       ↓
    Network + Compute + Storage
       ↓
    Application + Database
       ↓
    Load Balancer for traffic distribution

For our MERN project:

    React Client
        ↓
    Node / Express Server
        ↓
    MongoDB
        ↓
    Persistent Data

This local Docker architecture can be conceptually mapped to cloud infrastructure.

The next part will focus on protecting these resources through:

    IAM
    Authentication
    Authorization
    Roles
    Policies
    Least Privilege
    Network Security
    Secrets
    Public vs Private Resources


# Step 25 — Cloud Fundamentals

## Part 3 — Cloud Security & IAM

---

## 1. Overview

Cloud infrastructure is not secure simply because it is hosted by a cloud provider.

The application owner is still responsible for correctly configuring:

- Identity and access
- Permissions
- Network access
- Secrets
- Application security
- Data access

Cloud security therefore follows a shared responsibility model.

---

## 2. IAM

IAM = Identity and Access Management.

IAM controls:

    Who
      ↓
    Can access
      ↓
    Which resource
      ↓
    With which permissions

The main question is:

    "Who can do what?"

IAM is one of the core security mechanisms used in cloud environments.

---

## 3. Authentication

Authentication answers:

    "Who are you?"

Examples include:

- Username and password
- SSH keys
- Access credentials
- Identity provider authentication
- Multi-factor authentication

Conceptually:

    User
      ↓
    Authentication
      ↓
    Identity verified

Authentication happens before determining what that identity is allowed to do.

---

## 4. Authorization

Authorization answers:

    "What are you allowed to do?"

After an identity is verified, permissions determine which actions are allowed.

Conceptually:

    User
      ↓
    Authentication
      ↓
    Authorization
      ↓
    Allowed / Denied
      ↓
    Resource

Example:

    Developer
       ↓
    Authentication
       ↓
    Authorization
       ↓
    Read application logs
       ↓
    Allowed

The same user may not have permission to delete production infrastructure.

---

## 5. Authentication vs Authorization

| Concept | Main Question | Example |
|---|---|---|
| Authentication | Who are you? | Login / SSH key |
| Authorization | What can you do? | Read / Write / Delete |
| Authentication | Verifies identity | User identity |
| Authorization | Controls permissions | Resource access |

Simple rule:

    Authentication
        =
    Identity

    Authorization
        =
    Permission

---

## 6. IAM User

An IAM User represents an identity that can interact with cloud resources.

Conceptually:

    User
      ↓
    IAM
      ↓
    Cloud Resources

A user may be given permissions based on their responsibilities.

Example:

    Developer
       ↓
    IAM User
       ↓
    Specific permissions
       ↓
    Required resources

Users should not automatically receive unrestricted access.

---

## 7. IAM Role

A Role is an identity with a defined set of permissions that can be assumed or used by an appropriate identity or service.

Roles are especially useful for:

- Applications
- Cloud services
- Automation
- CI/CD systems
- Temporary access

Conceptually:

    Application
        ↓
    Role
        ↓
    Permissions
        ↓
    Cloud Resource

This avoids relying on unnecessary long-lived credentials.

---

## 8. IAM Policy

A Policy defines what actions are allowed or denied.

Conceptually:

    Identity / Role
          ↓
       Policy
          ↓
     Permissions
          ↓
       Resource

Example concept:

    Policy
      ├── Read
      └── Write

The exact policy syntax depends on the cloud provider.

---

## 9. Permission

A permission defines an allowed action on a resource.

Examples:

    Read
    Write
    Create
    Update
    Delete

A permission can be restricted to specific resources.

Example:

    Developer
        ↓
    Read
        ↓
    Application Logs

Instead of:

    Developer
        ↓
    Full Access
        ↓
    Entire Cloud Account

---

## 10. Principle of Least Privilege

The Principle of Least Privilege means:

    Give only the permissions required
    to perform the required task.

Example:

A developer only needs to read logs.

Correct:

    Developer
        ↓
    Read Logs
        ↓
    Application Logs

Not:

    Developer
        ↓
    Administrator
        ↓
    Entire Infrastructure

Least privilege reduces the potential impact of compromised credentials or accidental actions.

---

## 11. Least Privilege Example

| Requirement | Permission |
|---|---|
| View logs | Read logs |
| Deploy application | Deployment permissions |
| Read database | Database read |
| Modify database | Database write |
| Manage infrastructure | Infrastructure permissions |

The goal is to avoid giving permissions that are not required.

---

## 12. IAM Role vs User

| User | Role |
|---|---|
| Represents an identity | Represents a permission-based access identity |
| Often associated with a person | Often used by services/automation |
| Can have long-lived credentials | Can provide temporary/assumed access |
| Human access is a common use case | Application/CI/CD access is a common use case |

In modern DevOps systems, roles are important for reducing unnecessary credential exposure.

---

## 13. Security Groups

IAM controls identity and permissions.

Security Groups or equivalent network security mechanisms control network traffic.

Conceptually:

    Network Traffic
          ↓
    Security Rules
          ↓
    Resource

Rules can control things such as:

- Port
- Protocol
- Source
- Destination

---

## 14. IAM vs Security Group

| IAM | Security Group |
|---|---|
| Identity and permissions | Network traffic |
| Who can perform an action | Which traffic can reach a resource |
| Authentication/authorization layer | Network security layer |
| API/resource access | Port/network access |

Simple distinction:

    IAM
      ↓
    "What can you do?"

    Security Group
      ↓
    "What network traffic can reach it?"

---

## 15. Network Security Example

Suppose a backend server listens on port 5001.

A network security rule can determine whether traffic is allowed to reach that port.

Conceptually:

    Internet
       ↓
    Network Security Rule
       ↓
    Port 5001
       ↓
    Backend

For a database:

    Internet
       X
       │
    MongoDB

The database should generally not be directly exposed to the public Internet.

---

## 16. Public vs Private Resources

Not every cloud resource should be publicly accessible.

A common architecture is:

    Internet
       ↓
    Public Entry Point
       ↓
    Application
       ↓
    Private Database

The public entry point may be a load balancer or web-facing service.

The database remains inside a controlled private network.

---

## 17. Public Resource

A public resource can be reached from the public Internet when network and security rules allow it.

Examples may include:

- Public web application
- Public API endpoint
- Load balancer

Public access should be intentional.

---

## 18. Private Resource

A private resource is not directly exposed to the public Internet.

Examples may include:

- Database
- Internal service
- Internal management component

Conceptually:

    Public Internet
          ↓
    Public Application
          ↓
    Private Network
          ↓
    Private Database

This reduces unnecessary exposure.

---

## 19. Public vs Private

| Public | Private |
|---|---|
| Internet reachable | Internal access |
| Larger attack surface | Smaller exposure |
| Used for public services | Used for internal resources |
| Requires careful security rules | Access can be more restricted |

The goal is not to make everything private.

The goal is to expose only what actually needs public access.

---

## 20. Secrets

Secrets are sensitive values that should not be exposed publicly.

Examples:

- Database passwords
- API keys
- Access tokens
- Private keys
- Cloud credentials
- Authentication secrets

Secrets should not be hard-coded into source code.

---

## 21. Bad Secret Management

Example of an unsafe approach:

    const password = "real-password";

or:

    MONGO_PASSWORD=real-password

inside a committed source file.

This can expose credentials through:

- Git history
- Repository access
- Logs
- Build artifacts
- Shared code

---

## 22. Better Secret Management

A better approach is:

    Secret Store
         ↓
    Application / CI/CD
         ↓
    Runtime access

Secrets should be injected when required rather than permanently stored in application source code.

GitHub Actions also provides a mechanism for storing sensitive workflow values as repository/environment secrets.

---

## 23. Environment Variables and Secrets

Environment variables can separate configuration from application code.

Example concept:

    MONGO_URI
    API_KEY
    DATABASE_PASSWORD

The actual secret values should not be committed to Git.

A repository can contain a safe example:

    .env.example

while the real:

    .env

remains outside version control.

Our project already uses this principle.

---

## 24. Cloud Credentials

Cloud credentials provide access to cloud resources.

They must be protected carefully.

Bad practice:

    Source Code
        ↓
    Cloud Access Key
        ↓
    Git Repository

Better:

    Secure Credential / Role
        ↓
    CI/CD or Application
        ↓
    Required Cloud Resource

Credentials should receive only the permissions required for their purpose.

---

## 25. Shared Responsibility Model

Cloud security is shared between the cloud provider and the customer.

Conceptually:

    Cloud Provider
        ↓
    Security OF the Cloud

    Customer
        ↓
    Security IN the Cloud

The exact boundary depends on the service being used.

---

## 26. Provider Responsibilities

The provider generally manages infrastructure such as:

- Physical data centers
- Physical hardware
- Core cloud infrastructure
- Underlying facilities
- Infrastructure-level security

The exact responsibilities depend on the selected cloud service.

---

## 27. Customer Responsibilities

The customer is generally responsible for configuring and securing things such as:

- IAM
- Permissions
- Application code
- Secrets
- Data
- Network rules
- Operating system configuration when applicable
- Access policies

Therefore:

    Cloud Provider
        +
    Customer Configuration
        =
    Overall Security

---

## 28. MERN Security Mapping

Our MERN application can be mapped to cloud security concepts:

| MERN Component | Security Consideration |
|---|---|
| React Client | HTTPS / controlled public access |
| Node/Express Server | Authentication and authorization |
| MongoDB | Private network access |
| Docker | Secure image/configuration |
| `.env` | Secrets/configuration |
| GitHub Actions | Protected secrets |
| Cloud resources | IAM permissions |
| Network | Security rules |
| Database | Restricted access |

---

## 29. Security Architecture

A simplified secure architecture:

    Internet
        │
        ↓
    HTTPS
        │
        ↓
    Public Entry Point
        │
        ↓
    Backend
        │
        ↓
    Private Network
        │
        ↓
    MongoDB
        │
        ↓
    Persistent Storage

Alongside the architecture:

    IAM
      ↓
    Access Control

    Security Rules
      ↓
    Network Control

    Secret Management
      ↓
    Credential Protection

---

## 30. Security Principles

The most important principles from this part are:

### Principle 1 — Least Privilege

    Minimum required permissions

### Principle 2 — Do Not Expose Everything

    Public access only when required

### Principle 3 — Protect Secrets

    Never commit credentials

### Principle 4 — Separate Identity and Network Security

    IAM
      +
    Network Security

### Principle 5 — Assume Shared Responsibility

    Provider security
      +
    Customer configuration
      ↓
    Overall security

---

## 31. Practical Security Observation from Our Project

Our current Docker Compose configuration exposes:

    3000:80
    5001:5001
    27017:27017

The MongoDB port:

    27017:27017

means MongoDB is currently published to the host.

This is acceptable as a local development configuration when required for development/testing.

For a production cloud architecture, MongoDB should generally not be directly exposed to the public Internet.

Instead:

    Frontend
       ↓
    Backend
       ↓
    Private MongoDB

This demonstrates the difference between a convenient local development setup and a production-oriented security architecture.

---

## 32. Security Mental Model

A useful way to think about cloud security is:

    WHO?
      ↓
    IAM

    WHAT CAN THEY DO?
      ↓
    Policy / Permission

    CAN THEY REACH IT?
      ↓
    Network Security

    WHERE ARE THE CREDENTIALS?
      ↓
    Secret Management

    SHOULD IT BE PUBLIC?
      ↓
    Public / Private Architecture

---

## 33. Key Takeaways

| Concept | Meaning |
|---|---|
| IAM | Identity and access management |
| Authentication | Verifies identity |
| Authorization | Determines allowed actions |
| User | Identity representing a person |
| Role | Permission-based identity |
| Policy | Defines access rules |
| Permission | Allowed action |
| Least Privilege | Minimum required access |
| Security Group | Network traffic control |
| Secret | Sensitive credential/configuration |
| Public Resource | Internet-accessible resource |
| Private Resource | Internally accessible resource |
| Shared Responsibility | Provider + customer security responsibilities |

---

## 34. Part 3 Summary

Cloud security is based on controlling identity, permissions, network access, and sensitive information.

The main security model is:

    Identity
       ↓
    Authentication
       ↓
    Authorization
       ↓
    Permission
       ↓
    Resource

Network security adds another layer:

    Network Traffic
       ↓
    Security Rules
       ↓
    Resource

Sensitive credentials should be handled separately:

    Secret
       ↓
    Secure Storage
       ↓
    Application / CI/CD

The overall principle is:

    Least Privilege
        +
    Restricted Network Access
        +
    Protected Secrets
        +
    Correct IAM
        =
    Stronger Cloud Security



# Step 25 — Cloud Fundamentals

## Part 4 — Practical Lab, Cloud Mapping & Final Summary

---

## 1. Practical Lab Overview

The practical lab used the existing MERN Docker Compose project to connect the previously learned cloud concepts with a real application environment.

Project used:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

The goal was not to deploy the application to a paid cloud environment.

The goal was to observe the existing infrastructure and map it to cloud concepts.

---

## 2. Practical Lab Objective

The practical exercise focused on:

- Identifying application services
- Identifying exposed ports
- Identifying persistent storage
- Inspecting the MongoDB container
- Inspecting the Docker volume
- Understanding the MongoDB data directory
- Identifying public vs private architecture concerns
- Mapping local Docker infrastructure to cloud concepts

The practical approach was:

    Existing MERN Project
          ↓
    Docker Compose
          ↓
    Observe Infrastructure
          ↓
    Map to Cloud Concepts

---

## 3. Project Path

The practical project was located at:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

This is the downloaded MERN practice project used for DevOps learning.

It is separate from the DevOps documentation repository:

    /home/afroza/Projects/devops-learning

The DevOps learning documentation is maintained in the `devops-learning` repository.

---

## 4. Docker Compose Services

The MERN application contains three main services:

    client
    server
    mongo

Conceptually:

    MERN Application
          │
          ├── Client
          ├── Server
          └── MongoDB

The client represents the frontend.

The server represents the backend.

Mongo represents the database.

---

## 5. Application Relationship

The application architecture is:

    React Client
         ↓
    Node / Express Server
         ↓
    MongoDB

The frontend communicates with the backend.

The backend communicates with MongoDB.

The frontend does not need to communicate directly with MongoDB.

---

## 6. Port Mapping Observation

The Docker Compose configuration exposes:

    3000:80
    5001:5001
    27017:27017

These mappings were observed from the actual project configuration.

---

## 7. Client Port Mapping

The client uses:

    3000:80

This means:

    Host Port
        3000
          ↓
    Container Port
        80

Therefore the frontend is accessible through port 3000 on the host.

Conceptually:

    Host :3000
         ↓
    Client Container :80

---

## 8. Server Port Mapping

The server uses:

    5001:5001

This means:

    Host Port
        5001
          ↓
    Container Port
        5001

Therefore the backend can be accessed through port 5001 on the host.

Conceptually:

    Host :5001
         ↓
    Server Container :5001

---

## 9. MongoDB Port Mapping

MongoDB uses:

    27017:27017

This means:

    Host Port
        27017
          ↓
    MongoDB Container
        27017

This makes the MongoDB port available through the host.

This is useful to observe because it demonstrates the difference between a convenient local development configuration and a production-oriented cloud architecture.

---

## 10. Security Observation About MongoDB

The current local configuration publishes:

    27017:27017

Therefore MongoDB is exposed through the Docker host.

For local development, this can be intentional when direct database access is required.

For a production cloud architecture, the database should generally not be directly exposed to the public Internet.

A more appropriate architecture is:

    Internet
        ↓
    Frontend / Public Entry
        ↓
    Backend
        ↓
    Private MongoDB

This reduces unnecessary public exposure.

---

## 11. Docker Volume Observation

The Compose configuration contains:

    mongo-data

This is a named Docker volume.

The volume is associated with the MongoDB service.

Conceptually:

    MongoDB Container
          ↓
        /data/db
          ↓
      mongo-data
          ↓
    Persistent Data

This allows database data to live separately from the temporary container lifecycle.

---

## 12. Volume Inspection

The volume was inspected using:

    docker compose config --volumes

The output identified:

    mongo-data

This confirmed that the Compose project defines a named volume for MongoDB data.

---

## 13. MongoDB Container Inspection

The MongoDB service was inspected using:

    docker compose ps mongo

The MongoDB container was running successfully during the practical observation.

The container status showed that the MongoDB service was up.

This confirmed that the database container was active while the volume was inspected.

---

## 14. Mount Inspection

The MongoDB container mounts were inspected using Docker inspection.

The relevant mount information showed:

    Type: volume

and a source similar to:

    /var/lib/docker/volumes/blog-app-using-mern-stack_mongo-data/_data

The destination included:

    /data/db

and:

    /data/configdb

---

## 15. Important MongoDB Mount

The most important mapping observed was:

    Type: volume

    Source:
    /var/lib/docker/volumes/blog-app-using-mern-stack_mongo-data/_data

    Destination:
    /data/db

This means:

    Ubuntu Host
        ↓
    Docker Volume
        ↓
    mongo-data
        ↓
    MongoDB Container
        ↓
    /data/db

MongoDB therefore stores its database data through the persistent Docker volume.

---

## 16. Container vs Persistent Storage

The practical lab demonstrated an important distinction:

    Container
        ≠
    Persistent Storage

The MongoDB container is the application environment.

The `mongo-data` volume provides persistent storage.

Conceptually:

    MongoDB Container
          +
    Persistent Volume
          =
    Database with persistent data

The container lifecycle and the data lifecycle are therefore separated.

---

## 17. Why Persistent Storage Matters

Without persistent storage:

    MongoDB Container
          ↓
        Data
          ↓
    Container removed
          ↓
    Data may be lost

With persistent storage:

    MongoDB Container
          ↓
    Persistent Volume
          ↓
    Container removed
          ↓
    Volume remains
          ↓
    New MongoDB Container
          ↓
    Data can remain available

This is a fundamental infrastructure concept.

---

## 18. Local Docker → Cloud Concept Mapping

The practical lab allowed the local project to be mapped to cloud infrastructure.

| Local Project | Cloud Concept |
|---|---|
| Client container | Frontend compute/hosting |
| Server container | Backend compute |
| MongoDB container | Database |
| Docker network | Cloud network |
| `mongo-data` | Persistent storage concept |
| Port mapping | Network exposure |
| `.env` | Configuration/secrets |
| Nginx | Web server / reverse proxy |
| GitHub Actions | CI automation |

This is a conceptual mapping, not a claim that each Docker component is exactly equivalent to one specific cloud service.

---

## 19. Local Architecture

The current local architecture can be represented as:

    User
      │
      ↓
    Client
    :3000
      │
      ↓
    Server
    :5001
      │
      ↓
    MongoDB
    :27017
      │
      ↓
    mongo-data

This architecture is designed for local development and learning.

---

## 20. Production-Oriented Cloud Architecture

A more production-oriented conceptual architecture would be:

    Internet
        │
        ↓
    HTTPS / Public Entry
        │
        ↓
    Frontend
        │
        ↓
    Backend Compute
        │
        ↓
    Private Network
        │
        ↓
    Private MongoDB
        │
        ↓
    Persistent Storage

The exact cloud services used to implement this architecture depend on the selected cloud provider.

---

## 21. Public vs Private Mapping

The local project helped demonstrate why production architecture should distinguish between public and private resources.

### Local Development

    Client
      → Publicly accessible on host

    Server
      → Published port

    MongoDB
      → Published port

### Production-Oriented Concept

    Client / Public Entry
          ↓
       Backend
          ↓
    Private Database

The production design limits unnecessary Internet exposure.

---

## 22. Cloud Infrastructure Layers

The practical architecture can be viewed as:

    Region
      ↓
    Availability Zone
      ↓
    Network
      ↓
    Compute
      ↓
    Application
      ↓
    Database
      ↓
    Persistent Storage

Traffic management can be added through:

    Internet
      ↓
    Load Balancer
      ↓
    Application

These concepts were studied theoretically and then connected to the actual MERN project.

---

## 23. Practical Learning: Local to Cloud

The complete relationship is:

    Local Machine
         ↓
    Ubuntu
         ↓
    Docker
         ↓
    Docker Compose
         ↓
    MERN Application
         ↓
    Observe Infrastructure
         ↓
    Cloud Architecture Concepts

This creates a bridge between local DevOps practice and cloud infrastructure.

---

## 24. What Was Actually Practiced

The following were practically observed:

    Docker Compose services
        ↓
    Client
    Server
    MongoDB

    Port mappings
        ↓
    3000:80
    5001:5001
    27017:27017

    Docker volume
        ↓
    mongo-data

    MongoDB mount
        ↓
    /data/db
    /data/configdb

    Docker volume source
        ↓
    /var/lib/docker/volumes/...

These observations were taken from the actual running project environment.

---

## 25. What Was Not Practiced

No paid cloud infrastructure was created during Step 25.

The following were intentionally not performed:

- No production AWS deployment
- No paid cloud VM
- No production database deployment
- No real cloud credentials
- No production network creation
- No Terraform infrastructure
- No Kubernetes deployment

These topics belong to later stages of the roadmap.

---

## 26. Why Terraform Is Not Included Here

Terraform belongs to:

    Step 26 — Infrastructure as Code

Step 25 focuses on understanding:

    What infrastructure is required?

Step 26 will focus on:

    How can that infrastructure be defined and managed as code?

Therefore the progression is:

    Step 25
    Cloud Infrastructure Understanding
          ↓
    Step 26
    Terraform / Infrastructure as Code

This keeps the roadmap boundaries clear.

---

## 27. Step 25 Practical Lessons

### Lesson 1 — Containers Are Not Infrastructure

Docker packages and runs applications.

Cloud provides infrastructure and managed services.

They work together but solve different problems.

---

### Lesson 2 — Data Must Have Its Own Lifecycle

MongoDB data was separated from the container through:

    mongo-data

This demonstrates why persistent storage is necessary for stateful applications.

---

### Lesson 3 — Public Exposure Must Be Intentional

The local project exposes:

    27017:27017

This is useful for local development but should not automatically be copied into a production cloud architecture.

---

### Lesson 4 — Local and Production Architecture Differ

Local:

    Client
    Server
    MongoDB
    Docker Compose

Production:

    Public Entry
        ↓
    Application
        ↓
    Private Database
        ↓
    Persistent Storage

The production environment requires additional infrastructure and security considerations.

---

### Lesson 5 — Cloud Is More Than Compute

A cloud application requires multiple infrastructure layers:

    Compute
    Network
    Storage
    Database
    Security
    Availability

Thinking only about a cloud VM is not enough for production architecture.

---

## 28. Final MERN Cloud Architecture

The final conceptual architecture for our MERN application is:

    User
      │
      ↓
    Internet
      │
      ↓
    HTTPS / Public Entry
      │
      ↓
    Frontend
      │
      ↓
    Backend Compute
      │
      ↓
    Private Network
      │
      ↓
    MongoDB
      │
      ↓
    Persistent Storage

Supporting infrastructure:

    IAM
      ↓
    Access Control

    Network Security
      ↓
    Traffic Control

    Secret Management
      ↓
    Credential Protection

    GitHub Actions
      ↓
    CI Automation

---

## 29. Step 25 Knowledge Flow

The complete learning progression of Step 25 is:

    25.1
    Cloud Fundamentals
        ↓
    25.2
    Cloud Infrastructure
        ↓
    25.3
    Cloud Security & IAM
        ↓
    25.4
    MERN → Cloud Mapping
        ↓
    Practical Lab
        ↓
    Local Infrastructure → Cloud Architecture

The practical lab connected theoretical concepts to the actual Dockerized MERN project.

---

## 30. Step 25 Final Takeaways

The most important concepts learned in Step 25 are:

    Cloud
      ↓
    On-demand infrastructure/services

    Region
      ↓
    Geographic cloud location

    Availability Zone
      ↓
    Isolated infrastructure location

    Compute
      ↓
    Runs applications

    Network
      ↓
    Connects components

    Storage
      ↓
    Persists data

    Database
      ↓
    Stores application data

    IAM
      ↓
    Controls access

    Security
      ↓
    Protects infrastructure and data

    Docker
      ↓
    Packages application

    Cloud
      ↓
    Provides infrastructure

---

## 31. Step 25 Final Status

| Area | Status |
|---|---|
| Cloud Computing Fundamentals | ✅ |
| IaaS / PaaS / SaaS | ✅ |
| Region | ✅ |
| Availability Zone | ✅ |
| Compute | ✅ |
| Networking | ✅ |
| Storage | ✅ |
| Database | ✅ |
| Load Balancer | ✅ |
| IAM | ✅ |
| Authentication / Authorization | ✅ |
| Least Privilege | ✅ |
| Security Groups | ✅ |
| Secrets | ✅ |
| Public vs Private Resources | ✅ |
| MERN → Cloud Mapping | ✅ |
| Docker Volume Practical | ✅ |
| MongoDB Mount Practical | ✅ |
| Persistent Storage Observation | ✅ |
| Local → Cloud Architecture | ✅ |

---

# Step 25 — Cloud Fundamentals → COMPLETE

The practical and theoretical objectives of Step 25 have been completed.

The application has not been deployed to a real production cloud environment.

Instead, the existing Dockerized MERN application was used to understand how local application components map to cloud infrastructure and how production-oriented cloud architecture differs from local development.


