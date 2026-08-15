# Step 22 — Docker & Containerization
## Part 1 — Docker Fundamentals & Containerization

---

# 1. Part Overview

Step 22 introduces Docker and applies containerization to the MERN project.

This part focuses on the fundamental Docker concepts and the first practical containerization work performed on the Blog App project.

Project used for practical work:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

Documentation repository:

    /home/afroza/Projects/devops-learning

The practical work was performed on Ubuntu 26.04 LTS running inside VirtualBox.

---

# 2. Objectives

The objectives of this part were:

| Objective | Purpose |
|---|---|
| Verify Docker installation | Confirm Docker Engine is available |
| Understand Docker Engine | Understand client and server components |
| Understand Image | Learn the immutable application template concept |
| Understand Container | Learn the running instance concept |
| Inspect Docker environment | Understand the current Docker host |
| Inspect project structure | Identify the backend Docker configuration |
| Understand Dockerfile | Understand how the backend image is built |
| Build the backend image | Convert project configuration into a Docker image |
| Run the backend container | Run the Express application inside Docker |
| Verify container execution | Confirm that the application is actually running inside the container |

---

# 3. Docker Environment

The first verification was performed against the installed Docker environment.

Docker version:

    Docker version 29.7.2

Docker Compose plugin:

    Docker Compose v5.4.0

Docker service:

    active (running)

Docker command location:

    /usr/bin/docker

Current user belongs to the docker group:

    docker

---

# 4. Docker Engine Overview

Docker uses a client-server architecture.

The Docker CLI is used to send commands to the Docker daemon.

Workflow:

    Docker CLI
        |
        v
    Docker Daemon
        |
        +------------------+
        |                  |
        v                  v
    Images             Containers
                           |
                           v
                       Application

Important components observed in the environment:

| Component | Observed Value | Purpose |
|---|---|---|
| Docker Client | 29.7.2 | Sends Docker commands |
| Docker Server/Engine | 29.7.2 | Manages Docker resources |
| containerd | Installed | Container runtime management |
| runc | Installed | Runs containers |
| Storage Driver | overlayfs | Stores image/container layers |
| Cgroup Driver | systemd | Resource management |
| Cgroup Version | 2 | Resource control |
| Docker Root Directory | /var/lib/docker | Docker data storage |
| Architecture | x86_64 | Host architecture |
| CPUs | 2 | Docker host CPU allocation |
| Memory | 3.319 GiB | Docker host available memory |

---

# 5. Docker Engine vs Docker CLI

| Docker CLI | Docker Engine / Daemon |
|---|---|
| User-facing command tool | Background service |
| Receives commands from user | Executes Docker operations |
| Example: docker ps | Manages containers |
| Example: docker build | Manages images |
| Example: docker run | Manages networks and volumes |

The CLI does not itself run the container.

The command is sent to the Docker daemon, and the daemon performs the operation.

---

# 6. Docker Info

The Docker environment was inspected using Docker information commands.

Important observations from Docker info:

| Item | Value |
|---|---|
| Containers | 2 |
| Running | 1 |
| Stopped | 1 |
| Images | 2 |
| Storage Driver | overlayfs |
| Network Driver | bridge available |
| Docker Root Dir | /var/lib/docker |
| Swarm | inactive |
| Security | AppArmor, seccomp |
| Runtime | runc |
| Operating System | Ubuntu 26.04 LTS |

These values describe the Docker host rather than the MERN application itself.

---

# 7. Docker Image

A Docker image is a packaged template from which containers are created.

Conceptual relationship:

    Dockerfile
        |
        v
    Docker Image
        |
        v
    Container

The image contains the application environment and required filesystem layers.

An image itself is not the running application.

---

# 8. Docker Container

A container is a running instance created from an image.

Relationship:

    Image
       |
       +---- Container 1
       |
       +---- Container 2
       |
       +---- Container 3

The same image can be used to create multiple containers.

---

# 9. Image vs Container

| Image | Container |
|---|---|
| Template | Running instance |
| Used to create containers | Created from an image |
| Normally immutable | Has a writable runtime layer |
| Contains application filesystem | Runs the application process |
| Built using Dockerfile | Started using docker run / Compose |
| Can exist without running | Represents a running or stopped instance |

Simple mental model:

    Image = Blueprint

    Container = Running instance of that blueprint

---

# 10. Dockerfile

The backend project already contained a Dockerfile.

Backend project path:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack/server

The backend project contained:

| Item | Purpose |
|---|---|
| Dockerfile | Docker image build instructions |
| package.json | Node.js project dependencies and scripts |
| package-lock.json | Locked npm dependency versions |
| server.js | Express application entry point |
| node_modules | Installed dependencies |
| config | Application configuration |
| controller | Business logic |
| model | MongoDB/Mongoose models |
| routes | API routes |
| utils | Utility modules |

---

# 11. Backend Application

The backend is an Express application.

Backend port defined in the application:

    5001

The server starts using:

    npm start

The package script uses:

    nodemon server.js

The backend dependencies include Express, Mongoose, CORS, Helmet, dotenv and other application dependencies.

---

# 12. Why Containerize the Backend

Before Dockerization, the backend depended directly on the host environment.

Traditional flow:

    Ubuntu Host
        |
        +-- Node.js
        +-- npm
        +-- node_modules
        |
        v
      Express
        |
        v
      MongoDB

After containerization:

    Ubuntu Host
        |
        v
    Docker Engine
        |
        v
    Backend Container
        |
        +-- Node.js
        +-- Application
        +-- Dependencies
        |
        v
      Express

The goal is to make the application environment reproducible and isolated from the host.

---

# 13. Docker Image Build Workflow

The backend Dockerization workflow was:

    Backend Source Code
          |
          v
      Dockerfile
          |
          v
      docker build
          |
          v
      Docker Image
          |
          v
      docker run
          |
          v
      Backend Container
          |
          v
      Express :5001

Each stage has a different responsibility.

| Stage | Responsibility |
|---|---|
| Source Code | Application implementation |
| Dockerfile | Defines image build instructions |
| Build | Creates image |
| Image | Stores packaged application environment |
| Container | Runs the application |
| Port Mapping | Makes the container application reachable from host |

---

# 14. Important Docker Commands

The following commands were used during Step 22.

| Command | Purpose |
|---|---|
| docker --version | Show Docker CLI version |
| docker info | Show Docker Engine information |
| docker images | List local images |
| docker ps | List running containers |
| docker ps -a | List running and stopped containers |
| docker build | Build an image |
| docker run | Create and run a container |
| docker logs | View container application logs |
| docker exec | Execute a command inside a container |
| docker inspect | Show detailed Docker resource information |

---

# 15. Important Docker Flags

| Flag | Used With | Purpose |
|---|---|---|
| -d | docker run | Run container in detached/background mode |
| --name | docker run | Assign a custom container name |
| -p | docker run | Map host port to container port |
| -e | docker run | Provide an environment variable |
| -it | docker exec | Interactive terminal |
| -a | docker ps | Include stopped containers |
| --tail | docker logs | Limit the number of log lines |
| -f | docker logs | Follow live logs |
| -t | docker build | Assign image name/tag |
| -f | docker build | Specify Dockerfile |

---

# 16. Port Mapping

The backend application listens on:

    5001

Docker port mapping uses the format:

    HOST_PORT:CONTAINER_PORT

For the backend:

    5001:5001

Meaning:

| Side | Port |
|---|---|
| Host | 5001 |
| Container | 5001 |

Workflow:

    Host
      |
      | localhost:5001
      v
    Docker
      |
      | container :5001
      v
    Express

Port mapping does not change the port used by the Express application.

It connects a host port to a container port.

---

# 17. Container Verification

After the backend container was started, container status and application behavior were checked.

Important verification points:

| Verification | Purpose |
|---|---|
| docker ps | Confirm container is running |
| Port mapping | Confirm host can reach container |
| docker logs | Confirm Express started |
| docker exec | Inspect container internally |
| curl | Verify HTTP response |

The Express application successfully started inside the container.

Observed application message:

    app started at 5001...

---

# 18. Direct Backend Verification

The backend was also tested directly on port 5001.

The application returned an HTTP response.

When requesting the root path:

    /

Express returned:

    HTTP/1.1 404 Not Found

    Cannot GET /

This was not a Docker failure.

The response proved that:

1. The request reached Express.
2. Express was running.
3. The root route `/` was not defined.

The application API routes were mounted under:

    /api/users

    /api/blogs

Therefore, a 404 response from `/` does not mean the Express server is down.

---

# 19. Important Difference: Application Error vs Docker Error

| Situation | Meaning |
|---|---|
| Cannot connect to port | Possible container/port/network problem |
| Express returns 404 | Request reached Express, route may not exist |
| Express returns 500 | Application/backend error |
| Container is Exited | Container process stopped |
| Container is Up | Container process is running |
| Docker build fails | Image creation problem |
| Docker run fails | Container creation/startup problem |

This distinction became important during later troubleshooting.

---

# 20. What Was Learned in Part 1

| Concept | Practical Understanding |
|---|---|
| Docker Engine | Runs and manages containers |
| Docker CLI | Sends commands to Docker Engine |
| Image | Application/environment template |
| Container | Running instance of an image |
| Dockerfile | Instructions for building an image |
| Build | Converts Dockerfile + context into image |
| Run | Creates and starts container |
| Port Mapping | Connects host port to container port |
| Logs | Shows application/container output |
| Exec | Allows inspection inside container |
| HTTP 404 | Can mean application route is missing, not Docker failure |

---

# 21. Part 1 Workflow Summary

    Ubuntu Host
        |
        v
    Docker Engine
        |
        v
    Dockerfile
        |
        v
    Docker Build
        |
        v
    Backend Image
        |
        v
    Backend Container
        |
        | 5001:5001
        v
    Express Application
        |
        v
    HTTP Verification

This established the basic Docker workflow that was used for the remaining MERN containerization work.

---

# 22. Part 1 Completion Checklist

| Task | Status |
|---|---|
| Docker installed | Completed |
| Docker service verified | Completed |
| Docker Engine inspected | Completed |
| Docker CLI understood | Completed |
| Image concept understood | Completed |
| Container concept understood | Completed |
| Image vs Container compared | Completed |
| Backend Dockerfile identified | Completed |
| Backend image built | Completed |
| Backend container started | Completed |
| Port mapping verified | Completed |
| Express container verified | Completed |

---

# 23. Key Takeaway

The main practical workflow learned in Part 1 was:

    Dockerfile
        ->
    Image
        ->
    Container
        ->
    Port Mapping
        ->
    Running Express Application

The important distinction is:

    Image = packaged template

    Container = running instance

Docker therefore allows the MERN backend application and its runtime environment to be packaged separately from the Ubuntu host environment.

# Step 22 — Docker & Containerization
## Part 2 — Docker Networking & Backend ↔ MongoDB

---

# 1. Part Overview

Part 2 focuses on Docker container networking and the communication path between the MERN backend and MongoDB.

The main practical objective was to make the Express backend container communicate with the MongoDB container using Docker networking and container/service names instead of relying on localhost.

Project used for practical work:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

Documentation repository:

    /home/afroza/Projects/devops-learning

---

# 2. Objective

The objectives of this part were:

| Objective | Purpose |
|---|---|
| Understand Docker networks | Understand how containers communicate |
| Inspect existing networks | Identify which network each container uses |
| Identify the MongoDB network | Find the network where MongoDB is reachable |
| Connect backend to MongoDB network | Put both containers on a common network |
| Test Docker DNS | Verify MongoDB hostname resolution |
| Test MongoDB port | Verify TCP connectivity |
| Verify backend-to-database communication | Confirm actual network connectivity |
| Understand container hostname vs localhost | Avoid incorrect connection addresses |

---

# 3. Why Networking Was Required

The backend was running inside a Docker container.

MongoDB was also running inside a Docker container.

Therefore, the communication path was:

    Backend Container
          |
          v
    Docker Network
          |
          v
    MongoDB Container

The backend could not simply assume that its own localhost represented the MongoDB container.

Important rule:

    localhost / 127.0.0.1 inside a container
        =
    that same container

It does not automatically mean another container.

---

# 4. Host Networking vs Container Networking

| Address | Meaning |
|---|---|
| 127.0.0.1 | Current machine/container itself |
| localhost | Current machine/container itself |
| Host IP | Host machine address |
| Container IP | Specific container address |
| Container name | Docker DNS name for a container when available |
| Compose service name | Docker DNS name provided by Compose |

For backend-to-MongoDB communication, the preferred pattern is:

    mongodb://mongo:27017/BlogApp

or, depending on the actual container/network naming:

    mongodb://mongodb:27017/BlogApp

The hostname must be resolvable from the backend container.

---

# 5. Initial Networking Problem

During Dockerization, the backend attempted to connect to:

    mongo:27017

The backend logs showed:

    MongooseServerSelectionError: getaddrinfo ENOTFOUND mongo

Important meaning:

    ENOTFOUND
        =
    hostname could not be resolved

This was not initially a MongoDB authentication problem.

It was a Docker networking / hostname resolution problem.

---

# 6. Problem Analysis

At the time of the error, the backend and MongoDB were not participating in the same Docker network in the required way.

The backend container was associated with:

    bridge

MongoDB was associated with:

    mongodb_default

Therefore the backend could not resolve the MongoDB hostname through the Docker network it was using.

---

# 7. Network Inspection

The networks were inspected to determine where the containers were connected.

Important Docker commands:

    docker network ls

    docker network inspect <network-name>

The purpose of inspection was not just to see network names.

It was to identify:

| Information | Why it mattered |
|---|---|
| Network name | Identify the required network |
| Containers | See which containers belong to the network |
| IP addresses | Confirm container addressing |
| Aliases | Identify Docker DNS names |
| Driver | Understand network type |

---

# 8. Network Difference

The problematic state was conceptually:

    Backend
       |
       v
    bridge

    MongoDB
       |
       v
    mongodb_default

The required state was:

    Backend
       |
       |
       +----------------+
                        |
                        v
                 mongodb_default
                        |
                        |
                    MongoDB

Both containers needed to share a network for Docker DNS-based communication.

---

# 9. Connecting Backend to MongoDB Network

The backend container was connected to the MongoDB network.

The relevant operation was:

    docker network connect mongodb_default blog-backend-step22

The purpose was to attach the existing backend container to an additional Docker network.

---

# 10. Docker Network Connect

| Command | Purpose |
|---|---|
| docker network connect | Attach an existing container to a network |

Command structure:

    docker network connect NETWORK CONTAINER

In this case:

| Argument | Value |
|---|---|
| Network | mongodb_default |
| Container | blog-backend-step22 |

The important point is that this did not rebuild the backend image.

It changed the container's network attachments.

---

# 11. Backend Network Verification

After connecting the backend to the MongoDB network, the backend networks were inspected.

Observed result:

    Network=bridge
    Network=mongodb_default

This demonstrated that the backend was now attached to two networks.

| Network | Purpose |
|---|---|
| bridge | Existing/default container network |
| mongodb_default | Network shared with MongoDB |

A container can participate in multiple Docker networks.

---

# 12. MongoDB Network Verification

MongoDB was inspected and the MongoDB network showed the database container.

The MongoDB container was reachable through the Docker network.

The important concept was:

    Backend
       |
       | mongodb_default
       |
       v
    MongoDB

The shared network enabled Docker's internal name resolution and connectivity.

---

# 13. Docker DNS Verification

After fixing the network, hostname resolution was tested from inside the backend container.

Command used:

    docker exec blog-backend-step22 getent hosts mongodb

Observed result:

    172.18.0.2        mongodb  mongodb

This proved that the backend container could resolve the hostname:

    mongodb

to the MongoDB container IP:

    172.18.0.2

---

# 14. What getent hosts Proved

| Result | Meaning |
|---|---|
| Hostname returned | DNS resolution succeeded |
| IP returned | Docker resolved the container name |
| No result / error | Hostname resolution failed |

The important distinction:

    DNS resolution
        does not automatically prove
    application-level database connectivity

Therefore a second test was required.

---

# 15. MongoDB Port Verification

The MongoDB TCP port was tested from the backend container.

Command used:

    docker exec blog-backend-step22 sh -c 'nc -zv mongodb 27017 2>&1 || busybox nc -zv mongodb 27017 2>&1'

Observed result:

    mongodb (172.18.0.2:27017) open

This proved that the backend container could reach MongoDB on TCP port:

    27017

---

# 16. DNS vs Port Connectivity

| Test | What it proves |
|---|---|
| getent hosts mongodb | Hostname resolves to an IP |
| nc -zv mongodb 27017 | TCP port is reachable |
| Mongoose connection | Application can actually communicate with MongoDB |

These tests were intentionally separated.

A successful DNS lookup alone does not prove that the database port is accessible.

A successful TCP connection also does not necessarily prove that the application configuration is correct.

---

# 17. Direct Backend to MongoDB Flow

After networking was fixed, the communication path became:

    Backend Container
          |
          | mongodb
          v
    Docker DNS
          |
          | 172.18.0.2
          v
    MongoDB Container
          |
          | :27017
          v
    MongoDB

This was the first successful proof of container-to-container communication.

---

# 18. MongoDB Port

MongoDB uses:

    27017

There were two different concepts involved:

| Port | Purpose |
|---|---|
| Container port 27017 | MongoDB process inside the MongoDB container |
| Host port 27017 | Optional host access to MongoDB |

For backend-to-MongoDB communication inside the Docker network, the backend uses:

    mongodb:27017

It does not need to go through the host's published port.

---

# 19. Port Mapping vs Docker Network

| Port Mapping | Docker Network |
|---|---|
| Host to container access | Container to container access |
| Example: 5001:5001 | Example: mongodb:27017 |
| Used by host/browser | Used by backend/database |
| Depends on published host port | Uses Docker internal networking |
| Example: localhost:5001 | Example: mongo:27017 |

Important distinction:

    Host -> Container
        uses
    Port Mapping

    Container -> Container
        uses
    Docker Network

---

# 20. localhost vs Container Name

| Address | From Backend Container |
|---|---|
| 127.0.0.1 | Backend container itself |
| localhost | Backend container itself |
| mongodb | MongoDB container through Docker DNS |
| mongo | MongoDB Compose service/container name when defined that way |

Therefore:

    mongodb://127.0.0.1:27017/BlogApp

would attempt to reach MongoDB on the backend container itself.

Whereas:

    mongodb://mongodb:27017/BlogApp

targets the MongoDB container through Docker networking.

Later, with Docker Compose, the service name became:

    mongo

and the backend configuration became:

    mongodb://mongo:27017/BlogApp

---

# 21. Container Name vs Service Name

This distinction became important during the transition from manually managed containers to Docker Compose.

| Concept | Example | Meaning |
|---|---|---|
| Container name | mongodb | Name of a specific container |
| Compose service name | mongo | Name defined under services in Compose |
| Container IP | 172.x.x.x | Current container network address |
| Host port | 27017 | Published host port |
| Container port | 27017 | MongoDB process port |

With Docker Compose, service names are normally used for service-to-service communication.

Example:

    server
       |
       v
    mongo:27017

The IP address should not normally be hardcoded because container IP addresses can change.

---

# 22. Why Service Names Are Better Than Container IPs

| Service Name | Container IP |
|---|---|
| Stable logical name | Can change |
| Docker DNS resolves it | Must manually discover IP |
| Easy to understand | Less readable |
| Works well with Compose | Poor choice for hardcoded configuration |
| Supports container recreation | IP may change after recreation |

Therefore:

    mongodb://mongo:27017/BlogApp

is preferable to:

    mongodb://172.18.0.2:27017/BlogApp

---

# 23. Backend Port vs MongoDB Port

The backend and MongoDB have different responsibilities.

| Application | Port | Purpose |
|---|---:|---|
| Express | 5001 | HTTP API |
| MongoDB | 27017 | Database communication |

Flow:

    Browser / Host
         |
         v
    Express :5001
         |
         v
    MongoDB :27017

The backend does not expose MongoDB functionality.

It communicates with MongoDB internally.

---

# 24. Network Troubleshooting Workflow

When the backend could not connect to MongoDB, the troubleshooting process was:

    Connection Error
          |
          v
    Read application logs
          |
          v
    Identify ENOTFOUND
          |
          v
    Check network membership
          |
          v
    Check MongoDB hostname
          |
          v
    Attach backend to required network
          |
          v
    Test DNS
          |
          v
    Test TCP port
          |
          v
    Verify application connection

This avoided guessing and verified each networking layer separately.

---

# 25. Problem Faced: ENOTFOUND mongo

| Item | Details |
|---|---|
| Error | getaddrinfo ENOTFOUND mongo |
| Layer | Docker DNS / networking |
| Immediate meaning | Hostname could not be resolved |
| Root cause | Backend and MongoDB were not sharing the required network |
| Diagnostic | Network inspection |
| Fix | Connect backend to MongoDB network |
| Verification | getent hosts |
| Final result | Hostname resolved successfully |

---

# 26. Problem Faced: Backend and MongoDB on Different Networks

Initial state:

    Backend
       |
       v
    bridge

    MongoDB
       |
       v
    mongodb_default

Problem:

    Backend could not use MongoDB's Docker DNS name.

Solution:

    Backend
       |
       +-------------------+
                           |
                           v
                    mongodb_default
                           |
                           v
                       MongoDB

After the fix:

    Network=bridge
    Network=mongodb_default

---

# 27. Problem Faced: Port Connectivity

After DNS resolution was fixed, port connectivity was tested separately.

Observed:

    mongodb (172.18.0.2:27017) open

This confirmed:

    Backend
       |
       v
    MongoDB IP
       |
       v
    TCP 27017
       |
       v
    Reachable

This was stronger evidence than simply seeing the container as "running".

---

# 28. Verification Layers

The networking verification was performed at multiple layers.

| Layer | Test | Result |
|---|---|---|
| Container state | docker ps | Container running |
| Network membership | docker network inspect | Shared network confirmed |
| DNS | getent hosts mongodb | IP returned |
| TCP | nc -zv mongodb 27017 | Port open |
| Application | Mongoose connection | Connected |

This layered verification made it possible to identify whether a failure was caused by Docker, DNS, TCP connectivity, or application configuration.

---

# 29. Important Commands Used in Part 2

| Command | Purpose |
|---|---|
| docker network ls | List Docker networks |
| docker network inspect | Inspect network configuration |
| docker network connect | Attach container to a network |
| docker exec | Run command inside a container |
| getent hosts | Test hostname resolution |
| nc -zv | Test TCP port connectivity |
| docker ps | Verify running containers |
| docker logs | Inspect application errors |

---

# 30. Important Flags and Options

| Option | Command | Purpose |
|---|---|---|
| `--format` | docker inspect | Produce structured/custom output |
| `-z` | nc | Zero-I/O mode; scan port without sending data |
| `-v` | nc | Verbose output |
| `-c` | sh | Execute the supplied shell command |
| `-a` | docker ps | Include stopped containers |
| `-f` | docker logs | Follow live logs |

The exact command syntax should always be interpreted together with the command being used.

---

# 31. Docker Network vs Application Configuration

Networking alone was not sufficient.

There were three separate requirements:

    1. Containers must be on a compatible network.
    2. The hostname must resolve.
    3. The application must use the correct hostname and port.

Therefore:

    Network
       +
    DNS
       +
    Correct MONGO_URI
       =
    Successful Backend -> MongoDB Connection

This distinction became important later when environment-variable problems were diagnosed.

---

# 32. Final Network Architecture

After the networking problem was solved:

    Docker Host
         |
         v
    Docker Engine
         |
         v
    Docker Network
         |
         +-------------------+
         |                   |
         v                   v
    Backend Container    MongoDB Container
         |                   |
         | mongodb            | :27017
         |                   |
         +-------------------+
                 |
                 v
          MongoDB Database

The backend accessed MongoDB using the Docker network rather than using the host loopback address.

---

# 33. Part 2 Problem Summary

| # | Problem | Root Cause | Diagnosis | Solution | Result |
|---|---|---|---|---|---|
| 1 | `ENOTFOUND mongo` | Hostname unavailable from backend network | Application logs + network inspection | Shared Docker network | Resolved |
| 2 | Backend/MongoDB network mismatch | Different network membership | `docker network inspect` | `docker network connect` | Resolved |
| 3 | Hostname resolution initially failed | Docker DNS unavailable across networks | `getent hosts` | Same network | Resolved |
| 4 | MongoDB connectivity needed separate verification | DNS alone is insufficient | `nc -zv` | TCP test | Verified |

---

# 34. Key Differences Learned

## Docker Network vs Port Mapping

| Docker Network | Port Mapping |
|---|---|
| Container-to-container communication | Host-to-container communication |
| Internal Docker communication | External access from host |
| Uses container/service names | Uses host port |
| Example: mongo:27017 | Example: localhost:5001 |

## Container Name vs IP

| Container Name | IP |
|---|---|
| Logical identifier | Network address |
| Resolved by Docker DNS | May change |
| Easier to use | Should not normally be hardcoded |
| Preferred for configuration | Avoid hardcoding |

## localhost vs Container Name

| `localhost` | `mongo` / `mongodb` |
|---|---|
| Current container | Another container/service |
| Loopback | Docker DNS |
| Not suitable for MongoDB from backend container | Suitable when network is configured |

---

# 35. Part 2 Final Workflow

    Backend Container
          |
          | MONGO_URI
          v
    mongodb:27017
          |
          v
    Docker DNS
          |
          v
    Shared Docker Network
          |
          v
    MongoDB Container
          |
          v
    MongoDB Process :27017

Verification:

    Network membership
          ->
    DNS resolution
          ->
    TCP connectivity
          ->
    Application connection

---

# 36. Part 2 Completion Checklist

| Task | Status |
|---|---|
| Docker network concept understood | Completed |
| Existing networks inspected | Completed |
| Backend network membership inspected | Completed |
| MongoDB network identified | Completed |
| Backend connected to MongoDB network | Completed |
| Docker DNS tested | Completed |
| MongoDB hostname resolved | Completed |
| MongoDB port tested | Completed |
| Backend-to-MongoDB connectivity verified | Completed |
| localhost vs container hostname understood | Completed |
| Port mapping vs Docker networking understood | Completed |
| Network troubleshooting completed | Completed |

---

# 37. Key Takeaway

The main practical lesson from Part 2 was:

    Containers need a shared Docker network
    for reliable service-to-service communication.

The backend should not use:

    localhost:27017

to reach a MongoDB container.

Instead, when the containers share the appropriate Docker network, the backend can use:

    mongodb:27017

or, under the Compose configuration:

    mongo:27017

The important troubleshooting sequence was:

    Application Error
        ->
    Identify ENOTFOUND
        ->
    Inspect Docker Networks
        ->
    Fix Network Membership
        ->
    Verify DNS
        ->
    Verify TCP Port
        ->
    Verify Application Connectivity

This established the networking foundation required for the environment-variable, MongoDB persistence, and Docker Compose work documented in Parts 3 and 4.


# Step 22 — Docker & Containerization
## Part 3 — Environment Variables, MongoDB Persistence & Troubleshooting

---

# 1. Part Overview

Part 3 focuses on runtime configuration, environment variables, MongoDB persistence, and the practical troubleshooting performed during Dockerization of the MERN backend.

The main topics were:

    Environment Variables
        |
        +-- .env
        +-- Container Environment
        +-- process.env
        |
        v
    MONGO_URI
        |
        v
    Mongoose
        |
        v
    MongoDB

The second major topic was persistent MongoDB storage:

    MongoDB Container
        |
        v
    /data/db
        |
        v
    Docker Volume
        |
        v
    Persistent Data

Project used for practical work:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

Documentation repository:

    /home/afroza/Projects/devops-learning

---

# 2. Objectives

The objectives of this part were:

| Objective | Purpose |
|---|---|
| Understand `.env` | Separate runtime configuration from source code |
| Inspect container environment | Verify variables actually reach the container |
| Understand `process.env` | Understand how Node.js reads environment variables |
| Compare image and container environment | Understand build-time vs runtime configuration |
| Understand Docker Volume | Keep database data outside container lifecycle |
| Verify MongoDB persistence | Prove data survives container restart |
| Diagnose application errors | Separate Docker, environment, and application problems |
| Document troubleshooting | Build a repeatable debugging workflow |

---

# 3. Environment Variable Concept

The backend uses the MongoDB connection string through an environment variable.

The important variable was:

    MONGO_URI

The application reads it through Node.js:

    process.env.MONGO_URI

The practical flow was:

    .env
      |
      v
    Container Environment
      |
      v
    process.env.MONGO_URI
      |
      v
    Mongoose
      |
      v
    MongoDB

---

# 4. Host `.env`

The project contained a `.env` file.

The MongoDB connection value was stored in:

    .env

The actual secret value was intentionally not included in documentation output.

The environment variable was represented as:

    MONGO_URI=<hidden>

This avoids exposing database configuration values in documentation.

---

# 5. Container Environment Verification

The container environment was inspected without printing the secret value.

Important verification:

    MONGO_URI=<set>

This proved that the variable existed inside the container.

The Node.js runtime was also checked.

Observed result:

    MONGO_URI exists: true

This proved that the Node.js process could access the variable through:

    process.env.MONGO_URI

---

# 6. `.env` vs Container Environment vs process.env

| Layer | Example | Purpose |
|---|---|---|
| `.env` | MONGO_URI=<value> | Configuration source |
| Container Environment | MONGO_URI=<value> | Runtime variable inside container |
| Node.js | process.env.MONGO_URI | Application access |

Workflow:

    .env
      |
      v
    Docker runtime
      |
      v
    Container Environment
      |
      v
    process.env
      |
      v
    Application

These are related but represent different layers.

---

# 7. Image Environment vs Container Environment

The Docker image was inspected separately from the running container.

The image contained default Node-related environment variables such as:

    PATH
    NODE_VERSION
    YARN_VERSION

The container contained runtime configuration including:

    MONGO_URI

The important observation was that the MongoDB URI was not part of the image's default environment.

---

# 8. Image ENV vs Container ENV

| Image Environment | Container Environment |
|---|---|
| Defined as image defaults | Provided to a specific container |
| Part of image configuration | Runtime configuration |
| Shared by containers created from the image | Can differ between containers |
| Useful for defaults | Useful for deployment-specific configuration |
| Should not contain secrets | Secrets/configuration should be injected at runtime |

Mental model:

    Image
        =
    Application Template

    Container
        =
    Running Instance
    +
    Runtime Configuration

---

# 9. Runtime Environment Variable

A harmless runtime variable was added during verification:

    NODE_ENV=development

The container was recreated with:

    -e NODE_ENV=development

The result was:

    NODE_ENV=development

while the image itself did not contain NODE_ENV as one of its default environment variables.

This demonstrated that runtime configuration can be different from image configuration.

---

# 10. Important Docker Environment Options

| Option | Purpose |
|---|---|
| -e | Set an individual environment variable |
| --env-file | Load variables from an environment file |
| docker exec env | Inspect variables inside a running container |
| docker inspect | Inspect container configuration |

Important example:

    --env-file .env

This supplies values from the environment file to the container at runtime.

---

# 11. Why Secrets Should Not Be Hardcoded in Dockerfile

A secret such as a MongoDB password or connection URI should not be hardcoded directly into the Dockerfile.

Bad conceptual pattern:

    Dockerfile
        |
        v
    ENV MONGO_URI=<secret>

The preferred pattern is:

    Application Image
        |
        v
    Runtime Configuration
        |
        +-- .env
        +-- environment variable
        +-- deployment secret mechanism

The reason is separation between:

    Application Image

and:

    Environment-specific Configuration

The image can then be reused across environments without rebuilding it for every configuration change.

---

# 12. Environment Variable Troubleshooting

Environment variables became a major part of MongoDB connection troubleshooting.

The troubleshooting approach was:

    Application connection error
          |
          v
    Check MONGO_URI
          |
          v
    Check exact value
          |
          v
    Check whitespace
          |
          v
    Check URI scheme
          |
          v
    Correct environment value
          |
          v
    Recreate container
          |
          v
    Verify MongoDB connection

---

# 13. Problem: Invalid MongoDB URI

One MongoDB connection failure produced:

    MongoParseError:
    Invalid scheme

The error indicated that the MongoDB connection string was not being interpreted as a valid MongoDB URI.

The investigation focused on the actual environment value rather than assuming the Docker network was broken.

---

# 14. URI Validation

The MongoDB URI was checked character-by-character for formatting problems.

The checks included:

| Check | Purpose |
|---|---|
| URI value | See actual runtime value |
| Starts correctly | Verify `mongodb://` begins the URI |
| Whitespace | Detect unwanted leading/trailing spaces |
| Length | Confirm the value was not unexpectedly altered |

A diagnostic result showed:

    Has whitespace: true

and:

    Starts correctly: false

This identified formatting/whitespace as the cause of the URI parsing problem.

---

# 15. Problem: Leading Whitespace in MONGO_URI

The MongoDB URI contained unwanted leading whitespace.

The malformed value caused the MongoDB driver to reject the URI.

The corrected value was:

    mongodb://mongodb:27017/BlogApp

After correction, validation showed:

    Starts correctly: True

    Leading whitespace: 0

The backend was then recreated so that the corrected runtime environment was used.

---

# 16. Result After Environment Fix

After correcting the environment value and recreating the backend container, the backend logs showed:

    app started at 5001...

and:

    connected!

This demonstrated that:

    Environment Configuration
        +
    Docker Network
        +
    Correct MongoDB URI
        =
    Successful Mongoose Connection

---

# 17. Environment Troubleshooting Summary

| Problem | Root Cause | Diagnostic | Solution | Result |
|---|---|---|---|---|
| Invalid MongoDB URI | Malformed runtime value | Inspect exact environment value | Correct URI | Fixed |
| URI did not start correctly | Leading whitespace | Character-level validation | Remove whitespace | Fixed |
| Backend could not connect | Invalid configuration | Application logs | Recreate container with corrected environment | Fixed |

---

# 18. Docker Volume

The second major topic of Part 3 was persistent storage.

MongoDB stores its database files under:

    /data/db

A Docker Volume was attached to this location.

Conceptual flow:

    MongoDB Container
          |
          v
       /data/db
          |
          v
    Docker Named Volume
          |
          v
    Persistent Storage

---

# 19. Why a Volume Is Required for MongoDB

A container is replaceable.

Database data should not depend only on the container's writable layer.

Without a persistent volume:

    Container
        |
        v
    Database Data

If the container is removed, the data stored only in that layer may be lost.

With a volume:

    Container
        |
        v
    /data/db
        |
        v
    Docker Volume

The database data is stored outside the container's lifecycle.

---

# 20. Container Storage vs Docker Volume

| Container Writable Layer | Docker Volume |
|---|---|
| Belongs to container lifecycle | Exists independently |
| Temporary application storage | Persistent data storage |
| Can disappear when container is removed | Survives container removal unless explicitly deleted |
| Not ideal for database persistence | Appropriate for database persistence |
| Managed as part of container | Managed as Docker volume |

---

# 21. MongoDB Volume Verification

MongoDB mounts were inspected using Docker inspection.

The important destination was:

    /data/db

The MongoDB container had a Docker volume attached to this path.

The volume was also visible through:

    docker volume ls

The data directory inside MongoDB contained real database data.

Observed data size was approximately:

    225M

This showed that the volume was not merely configured; MongoDB was actively using it.

---

# 22. Important Volume Commands

| Command | Purpose |
|---|---|
| docker volume ls | List Docker volumes |
| docker volume inspect | Inspect volume metadata |
| docker inspect | Inspect container mounts |
| docker exec | Inspect data from inside container |

Important distinction:

    docker volume ls

shows volumes.

    docker inspect <container>

shows how a specific container uses the volume.

---

# 23. Persistence Test

A test document was inserted into MongoDB.

Test database:

    devops_test

Test collection:

    persistence

Test document:

    test: docker-volume

    created: step22.6

The document was then queried before and after restarting the MongoDB container.

---

# 24. First Persistence Test Issue

Immediately after restarting MongoDB, the first verification attempt produced:

    ECONNREFUSED 127.0.0.1:27017

This did not mean that the data had been lost.

The MongoDB container had restarted, but the MongoDB process had not yet completed startup.

---

# 25. Container Running vs Application Ready

This was an important practical distinction.

| Container State | Meaning |
|---|---|
| Container is Up | Container process is running |
| MongoDB ready | MongoDB is accepting connections |
| Application connected | Backend successfully established database connection |

A container can be running while the application inside it is still initializing.

Therefore:

    Container Up
        does not always mean
    Service Ready

---

# 26. MongoDB Startup Verification

MongoDB logs were inspected after the restart.

Important startup messages included:

    Listening on 0.0.0.0:27017

and:

    Waiting for connections

and:

    mongod startup complete

These messages showed that MongoDB had finished startup.

After waiting for readiness, the persistence query was executed again.

---

# 27. Persistence Test Result

The original document was successfully retrieved after the MongoDB restart:

    {
      test: 'docker-volume',
      created: 'step22.6'
    }

This proved:

    MongoDB Restart
        |
        v
    Volume Remounted
        |
        v
    /data/db Available
        |
        v
    Previous Data Available

---

# 28. Restart vs Data Loss

| Action | Volume Present | Expected Data |
|---|---|---|
| Container restart | Yes | Remains |
| Container stop/start | Yes | Remains |
| Container recreation | Yes | Remains |
| Container removal | Yes | Remains |
| Volume removal | No | Data lost |

Important:

    Container lifecycle
        is not the same as
    Volume lifecycle

---

# 29. Problem: ECONNREFUSED After Restart

| Item | Details |
|---|---|
| Error | ECONNREFUSED 127.0.0.1:27017 |
| Layer | Service readiness |
| Cause | MongoDB had not completed startup |
| Evidence | MongoDB startup logs |
| Fix | Wait until MongoDB was ready |
| Final result | Data successfully retrieved |

This problem was therefore classified as a startup/readiness issue rather than a persistence failure.

---

# 30. Problem: Persistence Test Returned No Data in Compose

Later, during Docker Compose verification, the persistence query initially returned:

    null

The reason was not that Docker Volume persistence was broken.

The earlier test data belonged to the previous standalone MongoDB volume.

Docker Compose created and used its own named volume:

    blog-app-using-mern-stack_mongo-data

Therefore, the old test document was not automatically present in the new Compose volume.

---

# 31. Standalone Volume vs Compose Volume

| Standalone Docker Work | Docker Compose |
|---|---|
| Earlier MongoDB container | Compose MongoDB service |
| Earlier volume | Compose named volume |
| Previous test data | New Compose test environment |
| Different lifecycle/configuration | Managed by Compose project |

This explained why the first Compose persistence lookup returned:

    null

The volume itself was working correctly.

---

# 32. Compose Persistence Test

A new test document was created specifically inside the Compose MongoDB volume.

Test document:

    test: compose-volume

    created: step22.10

Then the Compose MongoDB service was restarted.

Command:

    docker compose restart mongo

After restart, the same document was queried again.

---

# 33. Final Persistence Result

After restarting the Compose MongoDB service, the document was still present:

    {
      test: 'compose-volume',
      created: 'step22.10'
    }

This confirmed:

    Compose MongoDB
          |
          v
    /data/db
          |
          v
    Named Volume
          |
          v
    Restart
          |
          v
    Data Still Exists

---

# 34. Important Troubleshooting Distinctions

The following errors were deliberately separated during debugging.

| Error / Observation | Actual Meaning |
|---|---|
| ENOTFOUND | Hostname/DNS problem |
| Invalid scheme | MongoDB URI formatting problem |
| ECONNREFUSED | Service not accepting connection |
| HTTP 404 | Route does not exist |
| Container Exited | Main container process stopped |
| Container Up | Container process running |
| Persistence query null | Data not present in that specific database/volume |
| Compose version warning | Non-blocking configuration warning |

This classification prevents unrelated problems from being treated as the same Docker issue.

---

# 35. Problem Summary

| # | Problem | Category | Root Cause | Solution | Result |
|---|---|---|---|---|---|
| 1 | Invalid MongoDB URI | Environment | Malformed runtime value | Correct URI | Fixed |
| 2 | Leading whitespace in URI | Environment | Extra whitespace | Remove whitespace | Fixed |
| 3 | `ECONNREFUSED` after Mongo restart | Readiness | MongoDB still starting | Wait for readiness | Fixed |
| 4 | Compose persistence query returned `null` | Storage/Verification | Previous data belonged to different volume | Create test data in Compose volume | Fixed |
| 5 | Compose `version` warning | Configuration | Obsolete `version` attribute | Non-blocking; cleanup later | Not a failure |

---

# 36. Troubleshooting Workflow

The practical debugging workflow used in Part 3 was:

    Error
      |
      v
    Identify Layer
      |
      +-- Environment?
      |
      +-- Network?
      |
      +-- Service readiness?
      |
      +-- Storage?
      |
      +-- Application route?
      |
      v
    Inspect evidence
      |
      v
    Make targeted change
      |
      v
    Recreate/restart if required
      |
      v
    Verify again

The goal was to avoid changing multiple unrelated components at the same time.

---

# 37. Environment vs Network vs Storage

| Layer | Example Problem | Diagnostic |
|---|---|---|
| Environment | Invalid MONGO_URI | Inspect environment |
| Network | ENOTFOUND mongo | DNS/network inspection |
| Service Readiness | ECONNREFUSED | Service logs/status |
| Storage | Data missing | Volume/mount/data verification |
| Application | HTTP 404/500 | Express response/logs |

This layered approach made troubleshooting more systematic.

---

# 38. Important Commands Used

| Command | Purpose |
|---|---|
| docker exec | Run commands inside container |
| docker inspect | Inspect container configuration and mounts |
| docker volume ls | List volumes |
| docker volume inspect | Inspect volume |
| docker logs | Read MongoDB/backend logs |
| docker restart | Restart a container |
| docker compose restart | Restart a Compose service |
| env | Display environment variables |
| grep | Filter environment/configuration output |
| sed | Hide sensitive values from output |
| mongosh | Query MongoDB |
| sleep | Allow service startup time |

---

# 39. Important Options Used

| Option | Purpose |
|---|---|
| --quiet | Reduce mongosh output |
| --eval | Execute MongoDB JavaScript expression |
| --tail | Limit log output |
| --format | Format Docker inspection output |
| -f | Follow logs when used with docker logs |
| -e | Inject runtime environment variable |
| --env-file | Load environment variables from file |

---

# 40. Security Consideration

Environment values containing secrets were not printed directly in documentation.

Instead of exposing:

    MONGO_URI=<real-secret>

the verification used:

    MONGO_URI=<hidden>

or:

    MONGO_URI=<set>

This allows configuration verification without unnecessarily exposing credentials or connection information.

---

# 41. Final Environment Workflow

    .env
      |
      | runtime configuration
      v
    Docker Container
      |
      v
    MONGO_URI
      |
      v
    Node.js process.env
      |
      v
    Mongoose
      |
      v
    Docker Network
      |
      v
    MongoDB :27017
      |
      v
    /data/db
      |
      v
    Docker Volume

This connects the environment, networking, application, and persistence layers.

---

# 42. Part 3 Completion Checklist

| Task | Status |
|---|---|
| `.env` understood | Completed |
| Container environment inspected | Completed |
| process.env understood | Completed |
| Image ENV vs Container ENV compared | Completed |
| Runtime environment variable tested | Completed |
| MongoDB URI formatting debugged | Completed |
| Whitespace problem identified | Completed |
| Mongoose connection restored | Completed |
| Docker Volume identified | Completed |
| MongoDB `/data/db` verified | Completed |
| Persistence tested | Completed |
| Restart/readiness issue understood | Completed |
| Compose volume persistence verified | Completed |
| Troubleshooting categories documented | Completed |

---

# 43. Key Takeaway

Part 3 demonstrated that Docker problems should be diagnosed by layer.

    Environment
        |
        v
    Network
        |
        v
    Service Readiness
        |
        v
    Storage
        |
        v
    Application

A successful container deployment requires all of these layers to work together.

The most important practical lessons were:

    1. Runtime configuration should be separated from the image.

    2. process.env is how the Node.js application consumes runtime configuration.

    3. MongoDB data should use persistent storage.

    4. Container restart does not mean database data is lost when a volume is used.

    5. Container "Up" does not always mean the service is ready.

    6. A failed persistence query must be checked against the correct volume/database before declaring data loss.

    7. Docker troubleshooting should be evidence-based and layer-by-layer.


# Step 22 — Docker & Containerization
## Part 4 — Docker Compose, Full MERN Stack & Final Verification

---

# 1. Part Overview

Part 4 completes the Dockerization of the MERN project using Docker Compose.

The previous parts handled:

    Part 1
        Docker fundamentals
        Image
        Container
        Dockerfile
        Port mapping

    Part 2
        Docker networking
        Backend -> MongoDB communication
        Docker DNS
        TCP connectivity

    Part 3
        Environment variables
        MONGO_URI
        MongoDB persistence
        Docker volumes
        Troubleshooting

Part 4 combines these components into one reproducible MERN stack.

Project path:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

Documentation repository:

    /home/afroza/Projects/devops-learning

---

# 2. Objective

The objectives of Part 4 were:

| Objective | Purpose |
|---|---|
| Understand Docker Compose | Manage multiple containers as one application |
| Create Compose configuration | Define the MERN services |
| Define frontend service | Containerize React/Nginx |
| Define backend service | Containerize Node/Express |
| Define MongoDB service | Run MongoDB as a Compose service |
| Configure service dependencies | Define startup relationships |
| Configure environment variables | Connect backend to MongoDB |
| Configure named volume | Persist MongoDB data |
| Verify Compose network | Confirm service-to-service communication |
| Verify complete application | Confirm frontend/backend/database stack |
| Verify persistence | Confirm data survives MongoDB restart |

---

# 3. Why Docker Compose Was Needed

Before Compose, containers had to be managed individually.

Conceptually:

    docker run client
    docker run server
    docker run mongo

This becomes inconvenient when an application contains multiple services.

Docker Compose allows the complete application stack to be described in one configuration file.

Workflow:

    docker-compose.yaml
            |
            v
    Docker Compose
            |
            +------------------+
            |                  |
            v                  v
        Containers          Network
            |
            v
          Volume

---

# 4. Docker Compose File

The project used:

    docker-compose.yaml

The file defines three services:

    client
    server
    mongo

It also defines one named volume:

    mongo-data

---

# 5. Compose Service Architecture

The Compose configuration defines:

    client
        |
        v
    React + Nginx

    server
        |
        v
    Node + Express

    mongo
        |
        v
    MongoDB

Together:

    client
       |
       v
    server
       |
       v
    mongo

---

# 6. Compose Configuration

The actual service structure was:

    services:
      client:
        build:
          context: ./client
        ports:
          - "3000:80"
        depends_on:
          - server
        restart: always

      server:
        build:
          context: ./server
        ports:
          - "5001:5001"
        depends_on:
          - mongo
        environment:
          - MONGO_URI=mongodb://mongo:27017/BlogApp
        restart: always

      mongo:
        image: mongo
        ports:
          - "27017:27017"
        volumes:
          - mongo-data:/data/db
        restart: always

    volumes:
      mongo-data:

This configuration describes the complete application stack.

---

# 7. Service Comparison

| Service | Technology | Container Port | Host Port | Build/Image |
|---|---|---:|---:|---|
| client | React + Nginx | 80 | 3000 | Build from ./client |
| server | Node + Express | 5001 | 5001 | Build from ./server |
| mongo | MongoDB | 27017 | 27017 | mongo image |

---

# 8. Client Service

The client service uses:

    build:
      context: ./client

This means Docker builds the frontend image using the Dockerfile inside:

    ./client

Port mapping:

    3000:80

Meaning:

| Side | Port |
|---|---:|
| Host | 3000 |
| Container | 80 |

The browser therefore accesses:

    localhost:3000

while Nginx inside the container listens on:

    80

---

# 9. Server Service

The backend service uses:

    build:
      context: ./server

The backend container listens on:

    5001

Port mapping:

    5001:5001

The backend receives the MongoDB connection string through:

    MONGO_URI=mongodb://mongo:27017/BlogApp

Important observation:

    mongo

is the Compose service name.

Docker Compose provides DNS resolution between services.

Therefore the server can communicate with:

    mongo:27017

without knowing the MongoDB container IP.

---

# 10. Mongo Service

The MongoDB service uses the official MongoDB image:

    mongo

The MongoDB container listens on:

    27017

Port mapping:

    27017:27017

A named volume is mounted:

    mongo-data:/data/db

This keeps MongoDB database files outside the container lifecycle.

---

# 11. Compose Network

Docker Compose automatically created a default project network.

Observed network name:

    blog-app-using-mern-stack_default

All three services were connected to this network.

Conceptually:

    blog-app-using-mern-stack_default
             |
       +-----+-----+
       |     |     |
       v     v     v
     client server mongo

This allowed the services to communicate using service names.

---

# 12. Compose DNS

Inside the Compose network:

    server
       |
       | mongo
       v
    Docker DNS
       |
       v
    MongoDB service

The backend does not need to know:

    172.19.0.2

Instead it uses:

    mongo

Docker resolves the service name to the current container IP.

---

# 13. depends_on

The Compose file uses:

    depends_on:
      - server

for the client.

The server uses:

    depends_on:
      - mongo

The purpose is to express service startup dependency.

Relationship:

    client
       |
       v
    server
       |
       v
    mongo

Important distinction:

    depends_on
        does not automatically mean
    service is fully ready

It controls dependency/startup ordering behavior but does not replace application-level readiness checks.

---

# 14. restart: always

The services use:

    restart: always

This tells Docker to restart the container according to the configured restart policy if the container stops.

It improves service resilience during the local Docker setup.

---

# 15. Compose Volume

The Compose file defines:

    volumes:
      mongo-data:

The MongoDB service mounts:

    mongo-data:/data/db

This creates a named Docker volume associated with the Compose project.

Observed volume:

    blog-app-using-mern-stack_mongo-data

---

# 16. Named Volume Naming

Docker Compose automatically prefixes the project name to the volume.

Configuration name:

    mongo-data

Actual Docker volume:

    blog-app-using-mern-stack_mongo-data

This explains why the volume name seen from:

    docker volume ls

was longer than the name written in the Compose file.

---

# 17. docker compose config

Before running the full stack, the Compose configuration was rendered and checked using:

    docker compose config

The command showed:

    client
    server
    mongo

and the generated network and volume configuration.

This verified that Docker Compose could successfully interpret the YAML configuration.

---

# 18. Compose Warning

The configuration produced this warning:

    the attribute `version` is obsolete, it will be ignored,
    please remove it to avoid potential confusion

The file contained:

    version: "3.8"

The warning did not prevent the Compose project from running.

Classification:

| Item | Result |
|---|---|
| YAML parsing | Successful |
| Compose configuration | Successful |
| Services created | Successful |
| Warning | Non-blocking |
| Cause | Obsolete `version` attribute |

The warning can be cleaned up later by removing the obsolete version field.

It was not treated as a Docker runtime failure.

---

# 19. Compose Configuration vs Dockerfile

These two files have different responsibilities.

| Dockerfile | docker-compose.yaml |
|---|---|
| Defines how one image is built | Defines how multiple services run together |
| Image construction | Application orchestration |
| Base image | Services |
| Dependencies | Ports |
| Copy files | Networks |
| Build commands | Volumes |
| Container startup command | Environment variables |
| One application image | Complete application stack |

Simple distinction:

    Dockerfile
        =
    How to build an image

    Compose
        =
    How to run the application stack

---

# 20. Compose Commands Used

| Command | Purpose |
|---|---|
| docker compose config | Validate/render Compose configuration |
| docker compose up | Create and start services |
| docker compose ps | Show Compose service status |
| docker compose logs | Show service logs |
| docker compose exec | Execute command inside a service |
| docker compose restart | Restart a service |

---

# 21. Important Compose Options

| Option | Command | Purpose |
|---|---|---|
| -d | docker compose up | Run services in background |
| --build | docker compose up | Rebuild images before starting |
| --tail | docker compose logs | Limit log output |
| --quiet | mongosh | Reduce MongoDB shell output |
| --eval | mongosh | Execute an expression |
| -f | docker compose | Specify an alternative Compose file |

---

# 22. Starting the Full Stack

The Compose stack was started using Docker Compose.

The expected services were:

    client
    server
    mongo

The resulting containers were:

    blog-app-using-mern-stack-client-1
    blog-app-using-mern-stack-server-1
    blog-app-using-mern-stack-mongo-1

This demonstrated that Compose created all three application services.

---

# 23. Final Compose Status

The final Compose status showed:

    client    Up
    server    Up
    mongo     Up

Port mappings:

    client
        0.0.0.0:3000 -> 80

    server
        0.0.0.0:5001 -> 5001

    mongo
        0.0.0.0:27017 -> 27017

This confirmed that all three containers were running.

---

# 24. Frontend Verification

The frontend was tested through:

    localhost:3000

The response was:

    HTTP/1.1 200 OK

The response headers showed:

    Server: nginx

This proved:

    Host
      |
      v
    Port 3000
      |
      v
    Client Container
      |
      v
    Nginx
      |
      v
    React Application

The frontend was therefore successfully served from its Docker container.

---

# 25. Backend Verification

The backend was tested through:

    localhost:5001

The response was:

    HTTP/1.1 404 Not Found

with:

    Cannot GET /

This was not treated as a Docker failure.

The important observation was that the request reached Express successfully.

Therefore:

    Host
      |
      v
    Port 5001
      |
      v
    Server Container
      |
      v
    Express

The `/` route simply was not defined.

---

# 26. Backend Log Verification

The backend Compose logs showed:

    app started at 5001...

and:

    connected!

The two messages proved:

| Log | Meaning |
|---|---|
| app started at 5001 | Express started |
| connected! | Mongoose connected to MongoDB |

Therefore the backend was not only running; it was also connected to the database.

---

# 27. MongoDB Log Verification

MongoDB logs showed database activity and connections from the backend container.

Important observed behavior:

    Connection accepted

and client metadata identified:

    Node.js
    Mongoose

This provided evidence that the Node/Mongoose backend was actually communicating with MongoDB.

---

# 28. Backend to MongoDB DNS Verification

The backend service was used to test MongoDB DNS:

    docker compose exec server getent hosts mongo

Observed:

    172.19.0.2        mongo  mongo

This proved that the Compose service name:

    mongo

resolved successfully inside the server container.

---

# 29. Backend to MongoDB Port Verification

The MongoDB TCP port was tested from the backend service.

Command:

    docker compose exec server sh -c 'nc -zv mongo 27017 2>&1 || busybox nc -zv mongo 27017 2>&1'

Observed:

    mongo (172.19.0.2:27017) open

This proved:

    server
       |
       v
    mongo
       |
       v
    TCP 27017
       |
       v
    Open

---

# 30. Compose Persistence Verification

The MongoDB service used:

    mongo-data:/data/db

The volume was visible as:

    blog-app-using-mern-stack_mongo-data

A persistence test document was created:

    test: compose-volume
    created: step22.10

MongoDB was then restarted:

    docker compose restart mongo

After restart, the same document was retrieved.

Observed:

    {
      test: 'compose-volume',
      created: 'step22.10'
    }

This confirmed that the MongoDB data survived the service restart.

---

# 31. Why the First Compose Persistence Query Returned null

The first Compose persistence verification returned:

    null

This was investigated and found not to be a Docker Volume failure.

The previous persistence test belonged to the earlier standalone MongoDB environment.

Compose was using a different named volume:

    blog-app-using-mern-stack_mongo-data

Therefore the old test document was not expected to appear automatically.

The correct approach was:

    Create new test data
        |
        v
    Compose MongoDB
        |
        v
    Restart MongoDB
        |
        v
    Query same data
        |
        v
    Data still exists

---

# 32. Final MERN Architecture

The completed Docker Compose architecture was:

    Host Machine
         |
         v
    Docker Engine
         |
         v
    Docker Compose
         |
         +-------------------------------+
         |                               |
         v                               v
    Compose Network                 Named Volume
         |                           mongo-data
         |
         +-------------+-------------+
         |             |             |
         v             v             v
      Client         Server        Mongo
      Nginx          Express       MongoDB
       :80           :5001          :27017
         |             |
         |             |
     Host :3000     Host :5001
                       |
                       v
                    mongo:27017
                       |
                       v
                    MongoDB
                       |
                       v
                    /data/db
                       |
                       v
                    Volume

---

# 33. Complete Request and Data Flow

Frontend request flow:

    Browser
       |
       v
    localhost:3000
       |
       v
    Client Container
       |
       v
    Nginx
       |
       v
    React Application

Backend request flow:

    Client
       |
       v
    Server Container
       |
       v
    Express :5001
       |
       v
    Mongoose
       |
       v
    mongo:27017
       |
       v
    MongoDB

Persistence flow:

    MongoDB
       |
       v
    /data/db
       |
       v
    mongo-data
       |
       v
    Docker Named Volume

---

# 34. Host Port vs Container Port

The final application used three host/container mappings.

| Service | Host Port | Container Port | Access |
|---|---:|---:|---|
| Client | 3000 | 80 | Browser |
| Server | 5001 | 5001 | API |
| MongoDB | 27017 | 27017 | Host/database tools |

Important distinction:

    3000:80

means:

    Host 3000 -> Container 80

while:

    5001:5001

means:

    Host 5001 -> Container 5001

---

# 35. Compose Service Communication

Inside the Compose network:

    client
       |
       v
    server
       |
       v
    mongo

The service names are used instead of hardcoded IP addresses.

Backend MongoDB URI:

    mongodb://mongo:27017/BlogApp

This is more reliable than using:

    mongodb://172.19.0.2:27017/BlogApp

because container IP addresses can change when containers are recreated.

---

# 36. Final Verification Matrix

| Layer | Verification | Expected | Result |
|---|---|---|---|
| Docker Engine | Docker service | Active | Completed |
| Compose config | docker compose config | Valid configuration | Completed |
| Client | localhost:3000 | HTTP 200 | Completed |
| Server | localhost:5001 | Express response | Completed |
| Server logs | docker compose logs server | `connected!` | Completed |
| MongoDB | Mongo logs | Database running | Completed |
| DNS | getent hosts mongo | IP returned | Completed |
| TCP | nc mongo 27017 | Port open | Completed |
| Compose status | docker compose ps | 3 services Up | Completed |
| Volume | docker volume ls | Named volume exists | Completed |
| Persistence | Restart MongoDB | Data retained | Completed |

---

# 37. Final Problem Summary for Step 22

All major practical problems encountered throughout Step 22 are summarized below.

| # | Problem | Category | Root Cause | Solution | Status |
|---|---|---|---|---|---|
| 1 | Unexpected token `export` | Application | CommonJS/ES Module mismatch | Use matching module syntax | Solved |
| 2 | Backend port mismatch | Docker | Incorrect host/container mapping | Correct `5001:5001` mapping | Solved |
| 3 | `ENOTFOUND mongo` | Networking | Backend and MongoDB not on required shared network | Connect backend to MongoDB network | Solved |
| 4 | Mongo hostname mismatch | Networking | Container/service naming difference | Use correct Docker hostname/service name | Solved |
| 5 | Invalid MongoDB URI | Environment | Malformed runtime URI | Correct MONGO_URI | Solved |
| 6 | URI leading whitespace | Environment | Extra whitespace | Remove whitespace | Solved |
| 7 | `ECONNREFUSED` after Mongo restart | Readiness | MongoDB still starting | Wait for service readiness | Solved |
| 8 | Compose persistence returned `null` | Storage/Verification | Different volume from previous test | Create test data in Compose volume | Solved |
| 9 | Compose `version` warning | Compose config | Obsolete version attribute | Non-blocking; can remove later | Non-blocking |

---

# 38. Final Docker Workflow

The complete workflow developed through Step 22 was:

    MERN Source Code
          |
          v
    Dockerfiles
          |
          v
    Docker Images
          |
          v
    Docker Compose
          |
          +-----------------------------+
          |                             |
          v                             v
      Containers                    Network
          |                             |
          |                   +---------+---------+
          |                   |                   |
          v                   v                   v
       Client              Server              Mongo
          |                   |                   |
          |                   |                   |
          |                   +------ mongo -----+
          |                                       |
          |                                       v
          |                                  /data/db
          |                                       |
          |                                       v
          |                                  Named Volume
          |
          v
       Browser

---

# 39. Dockerfile vs Image vs Container vs Compose

| Component | Main Responsibility |
|---|---|
| Dockerfile | Defines how an image is built |
| Image | Packages application environment |
| Container | Runs the packaged application |
| Docker Network | Connects containers |
| Docker Volume | Persists data |
| Docker Compose | Defines and manages multiple services |

This distinction is central to the entire Step 22 workflow.

---

# 40. What Was Actually Built

The practical result of Step 22 was not just a collection of Docker commands.

The MERN project was transformed into a multi-container application:

    React Frontend
          |
          v
    Nginx Container
          |
          v
    Node/Express Backend Container
          |
          v
    MongoDB Container
          |
          v
    Persistent Docker Volume

Docker Compose became the mechanism that defined the relationship between these components.

---

# 41. Final Step 22 Verification

The final state was:

    client
        Up
        3000 -> 80

    server
        Up
        5001 -> 5001
        connected to MongoDB

    mongo
        Up
        27017 -> 27017

    network
        blog-app-using-mern-stack_default

    volume
        blog-app-using-mern-stack_mongo-data

All three services were running together under Docker Compose.

---

# 42. Step 22 Completion Checklist

| Area | Status |
|---|---|
| Docker Engine | Completed |
| Docker CLI | Completed |
| Docker Image | Completed |
| Docker Container | Completed |
| Dockerfile | Completed |
| Port Mapping | Completed |
| Docker Networking | Completed |
| Container DNS | Completed |
| Backend -> MongoDB | Completed |
| Environment Variables | Completed |
| MongoDB Volume | Completed |
| MongoDB Persistence | Completed |
| Docker Compose | Completed |
| MERN multi-container stack | Completed |
| Frontend verification | Completed |
| Backend verification | Completed |
| MongoDB verification | Completed |
| Final end-to-end verification | Completed |

---

# 43. Final Concept Map

    Docker
      |
      +-- Image
      |     |
      |     +-- Container
      |
      +-- Network
      |     |
      |     +-- Client
      |     +-- Server
      |     +-- Mongo
      |
      +-- Volume
      |     |
      |     +-- MongoDB data
      |
      +-- Compose
            |
            +-- client
            +-- server
            +-- mongo
            +-- network
            +-- volume

---

# 44. Final Key Takeaways

| Concept | Practical Understanding |
|---|---|
| Docker Image | Reusable application template |
| Container | Running application instance |
| Port Mapping | Host access to container |
| Docker Network | Container-to-container communication |
| Docker DNS | Service/container name resolution |
| Environment Variable | Runtime application configuration |
| Volume | Persistent storage |
| Compose | Multi-container application orchestration |
| depends_on | Defines service dependency |
| restart | Defines container restart behavior |
| Service Name | Stable Compose communication name |
| Persistence | Data survives container restart when stored in volume |
| Troubleshooting | Diagnose by layer instead of guessing |

---

# 45. Final Step 22 Summary

Step 22 demonstrated the complete containerization workflow for the MERN project.

The application moved from a host-dependent setup to a multi-container Docker environment.

Final architecture:

    React + Nginx
          |
          v
    Node + Express
          |
          v
    MongoDB
          |
          v
    Docker Named Volume

Docker Compose manages the complete stack.

The final verification confirmed:

    Frontend reachable
    Backend reachable
    Backend connected to MongoDB
    Docker DNS working
    MongoDB port reachable
    All Compose services running
    MongoDB volume active
    Data surviving MongoDB restart

Therefore:

    Step 22 — Docker & Containerization
    Status: COMPLETED
