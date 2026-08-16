# Step 23 — Docker Compose

## Part 1 — Docker Compose Fundamentals

---

## 1. What is Docker Compose?

Docker Compose is a tool used to define and run a multi-container application from a single YAML configuration file.

Instead of manually creating and connecting multiple containers with separate `docker run` commands, Compose allows us to define the complete application stack in one file.

For our MERN application, the stack contains three services:

- Client — React frontend
- Server — Node.js/Express backend
- Mongo — MongoDB database

The complete stack can then be managed using Docker Compose commands.

---

## 2. Docker vs Docker Compose

| Docker | Docker Compose |
|---|---|
| Mainly manages individual containers | Manages multiple related containers |
| `docker run` | `docker compose up` |
| Network configuration may be manual | Compose creates a project network automatically |
| Environment variables passed manually | Defined inside Compose file |
| Volumes configured per container | Volumes defined centrally |
| Container-by-container management | Entire application stack management |
| Suitable for individual containers | Suitable for multi-service applications |

### Our project

Without Compose:

    docker run ...
    docker run ...
    docker run ...

With Compose:

    docker compose up -d

One command can start the complete MERN stack.

---

## 3. Compose Configuration File

Our project uses:

    docker-compose.yaml

Location:

    Blog-App-using-MERN-stack/docker-compose.yaml

The file defines the three application services:

    client
    server
    mongo

---

## 4. Current Docker Compose File

The Compose configuration used in this project:

    version: "3.8"

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

---

## 5. Compose File Structure

The main structure is:

    services:
      client:
      server:
      mongo:

    volumes:
      mongo-data:

### Main sections

| Section | Purpose |
|---|---|
| `services:` | Defines application containers/services |
| `client:` | React frontend service |
| `server:` | Node.js/Express backend service |
| `mongo:` | MongoDB database service |
| `volumes:` | Defines persistent Docker volumes |

---

## 6. Client Service

The client service builds the frontend image from the `client` directory.

    client:
      build:
        context: ./client

### `build`

`build` tells Compose to build an image from a Dockerfile.

### `context`

    context: ./client

This means the Docker build context is the project's `client` directory.

### Port mapping

    ports:
      - "3000:80"

Meaning:

    Host port 3000 → Container port 80

The React frontend is served through Nginx inside the container on port 80.

From the Ubuntu host:

    http://localhost:3000

---

## 7. Server Service

The server service contains the Node.js/Express backend.

    server:
      build:
        context: ./server

The backend image is built using the Dockerfile inside:

    ./server

### Port mapping

    ports:
      - "5001:5001"

Meaning:

    Host port 5001 → Container port 5001

Our Express application runs inside the container on port 5001.

### Environment variable

    environment:
      - MONGO_URI=mongodb://mongo:27017/BlogApp

This provides the MongoDB connection string to the backend container.

Important point:

The backend does NOT use:

    localhost:27017

Inside the Compose network, it uses:

    mongo:27017

because `mongo` is the Compose service name.

---

## 8. Mongo Service

The MongoDB service uses the official MongoDB image.

    mongo:
      image: mongo

Unlike `client` and `server`, we do not build this image from our project source.

Docker pulls the MongoDB image when necessary.

### Port mapping

    ports:
      - "27017:27017"

Meaning:

    Host port 27017 → Container port 27017

This allows MongoDB to be accessed from the host.

---

## 9. MongoDB Volume

MongoDB uses:

    volumes:
      - mongo-data:/data/db

Here:

    mongo-data

is the Docker named volume.

And:

    /data/db

is MongoDB's data directory inside the container.

This means MongoDB data is stored outside the container's writable layer.

Therefore, removing/recreating the MongoDB container does not automatically remove the database data.

The volume is declared at the bottom:

    volumes:
      mongo-data:

---

## 10. `depends_on`

Our Compose configuration contains:

    client:
      depends_on:
        - server

and:

    server:
      depends_on:
        - mongo

This expresses the application dependency chain:

    client
       ↓
    server
       ↓
    mongo

So the application has the following logical structure:

    React Client
         ↓
    Express Server
         ↓
      MongoDB

`depends_on` controls service startup dependency. It does not itself guarantee that the application inside the dependency is fully ready to accept requests.

---

## 11. `restart: always`

Each service has:

    restart: always

Example:

    server:
      restart: always

This tells Docker to restart the container when it stops according to Docker's restart-policy behavior.

Our three services therefore have the same restart policy:

| Service | Restart Policy |
|---|---|
| Client | `always` |
| Server | `always` |
| Mongo | `always` |

---

## 12. Compose Project Network

Docker Compose automatically creates a project network for the services.

Our project generated a network similar to:

    blog-app-using-mern-stack_default

The services can communicate with each other using their Compose service names.

For example:

    server → mongo:27017

The backend does not need to know MongoDB's container IP address.

Docker's internal DNS resolves:

    mongo

to the MongoDB container.

---

## 13. Important Compose Commands

### Check Compose configuration

    docker compose config

This validates and renders the effective Compose configuration.

### Show running Compose services

    docker compose ps

### Start the complete stack

    docker compose up -d

Important flag:

    -d

means detached mode, so the containers run in the background.

### Stop and remove the Compose stack

    docker compose down

This removes the Compose containers and network.

Without `-v`, named volumes are not removed.

### Restart the Compose stack

    docker compose restart

### View service logs

    docker compose logs

For a specific service:

    docker compose logs server

For recent logs:

    docker compose logs --tail 20 server

---

## 14. Compose Configuration Verification

We verified the Compose configuration using:

    docker compose config

The effective configuration showed:

    client
    server
    mongo

and the Compose-created network and volume.

This confirmed that Docker Compose could successfully parse the configuration.

---

## 15. Compose Version Warning

During our practical work, Docker Compose repeatedly showed:

    the attribute `version` is obsolete, it will be ignored

The current file contains:

    version: "3.8"

Modern Docker Compose v2 does not require this `version` field.

Important:

This is a warning, not a runtime failure.

The Compose stack continued to work correctly despite the warning.

For future cleanup, the obsolete `version` line can be removed.

---

## 16. Step 23 Part 1 Summary

Docker Compose allows our entire MERN application to be described as one multi-service stack.

Our architecture:

    client
       ↓
    server
       ↓
    mongo

Main Compose concepts learned in this part:

| Concept | Purpose |
|---|---|
| `services` | Define application services |
| `build` | Build custom images |
| `context` | Define Docker build context |
| `image` | Use an existing Docker image |
| `ports` | Map host and container ports |
| `environment` | Provide runtime configuration |
| `depends_on` | Define service dependencies |
| `restart` | Define container restart policy |
| `volumes` | Persist database data |
| Compose network | Allow service-to-service communication |


# Step 23 — Docker Compose

## Part 2 — Compose Networking & Service Communication

---

## 1. Docker Compose Network

When Docker Compose starts the project, it automatically creates a dedicated network for the Compose project.

Our project network:

    blog-app-using-mern-stack_default

Network driver:

    bridge

Scope:

    local

The three application services are connected to this network:

    client
    server
    mongo

This allows the containers to communicate with each other using Docker's internal networking.

---

## 2. Compose Network Architecture

Our application communication flow is:

    Client
       ↓
    Server
       ↓
    MongoDB

More specifically:

    client → server:5001
    server → mongo:27017

The important point is that containers communicate through the Compose network rather than through the host's `localhost`.

---

## 3. Service Name as DNS Name

Docker Compose provides internal DNS resolution for services.

Our MongoDB service is named:

    mongo

Therefore, from the server container, the hostname:

    mongo

resolves to the MongoDB container.

We verified this using:

    docker compose exec server getent hosts mongo

The result showed:

    172.19.0.4    mongo    mongo

This proves that Docker's internal DNS resolved the service name `mongo` to the MongoDB container IP.

---

## 4. Why `mongo` Instead of `localhost`?

Inside the server container:

    localhost

means:

    the server container itself

It does NOT mean the MongoDB container.

Therefore this would be incorrect for Compose service-to-service communication:

    mongodb://localhost:27017/BlogApp

The correct URI is:

    mongodb://mongo:27017/BlogApp

because:

    mongo

is the Docker Compose service name of the MongoDB container.

---

## 5. MongoDB Port Communication

MongoDB listens on:

    27017

inside the MongoDB container.

From the server container, we tested:

    docker compose exec server sh -c 'nc -zv mongo 27017 2>&1 || busybox nc -zv mongo 27017 2>&1'

Result:

    mongo (172.19.0.4:27017) open

This confirmed that the server container could reach MongoDB on TCP port `27017`.

---

## 6. DNS vs Port Test

We performed two different tests.

| Test | What it verifies | Result |
|---|---|---|
| `getent hosts mongo` | DNS/service-name resolution | Successful |
| `nc -zv mongo 27017` | TCP connectivity to MongoDB | Successful |

Therefore:

    DNS resolution ✓
    Network connectivity ✓

This confirms that the Compose network was functioning correctly.

---

## 7. Server `MONGO_URI`

The server service receives:

    MONGO_URI=mongodb://mongo:27017/BlogApp

The structure is:

    mongodb://
        ↓
    mongo
        ↓
    27017
        ↓
    BlogApp

| Part | Meaning |
|---|---|
| `mongodb://` | MongoDB connection scheme |
| `mongo` | Compose service/DNS name |
| `27017` | MongoDB port |
| `BlogApp` | Database name |

The backend therefore connects to MongoDB through the internal Compose network.

---

## 8. Host Port vs Container Port

Our Compose configuration contains:

    server:
      ports:
        - "5001:5001"

    mongo:
      ports:
        - "27017:27017"

    client:
      ports:
        - "3000:80"

The format is:

    HOST:CONTAINER

Therefore:

| Service | Host Port | Container Port |
|---|---:|---:|
| Client | 3000 | 80 |
| Server | 5001 | 5001 |
| Mongo | 27017 | 27017 |

---

## 9. Internal vs External Communication

There are two different networking paths.

### Host → Container

From Ubuntu host:

    localhost:3000
    localhost:5001
    localhost:27017

These use the published host ports.

### Container → Container

Inside the Compose network:

    server → mongo:27017

This uses the service name and the container port.

The backend does not need the host port mapping to communicate with MongoDB.

---

## 10. Important Networking Difference

Example:

    Host → MongoDB

uses:

    localhost:27017

But:

    Server container → MongoDB container

uses:

    mongo:27017

This distinction is important in Dockerized applications.

| Connection | Address |
|---|---|
| Ubuntu host → MongoDB | `localhost:27017` |
| Server → MongoDB | `mongo:27017` |
| Browser → Frontend | `localhost:3000` |
| Browser → Backend | `localhost:5001` |

---

## 11. Verifying the Compose Network

The project network can be inspected with:

    docker network ls

Our Compose network appeared as:

    blog-app-using-mern-stack_default

We can inspect the network with:

    docker network inspect blog-app-using-mern-stack_default

This can show the containers attached to the network and their network configuration.

---

## 12. Service Communication Verification Workflow

The networking verification workflow used in this project was:

    1. Check Compose services
       ↓
    2. Verify Compose network
       ↓
    3. Resolve `mongo` from server
       ↓
    4. Test MongoDB port
       ↓
    5. Verify MONGO_URI
       ↓
    6. Verify backend connection

Commands used:

    docker compose ps

    docker compose exec server getent hosts mongo

    docker compose exec server sh -c 'nc -zv mongo 27017 2>&1 || busybox nc -zv mongo 27017 2>&1'

    docker compose exec server sh -c 'printf "%s\n" "$MONGO_URI" | sed "s#mongodb://#mongodb://<hidden>@#"'

---

## 13. Why This Verification Matters

A backend may be running but still be unable to communicate with MongoDB.

Possible failure points include:

    Service name resolution
    Network connection
    Port availability
    Incorrect MongoDB URI
    MongoDB not running

Our verification showed:

    Server container running        ✓
    Mongo service running           ✓
    `mongo` DNS resolution          ✓
    MongoDB port 27017 reachable    ✓
    MONGO_URI configured            ✓
    Mongoose connection             ✓

Therefore the server-to-MongoDB communication path was working.

---

## 14. Compose Network Mental Model

Think of the Compose network as a private network for the application.

    ┌──────────────────────────────────────┐
    │ Docker Compose Network               │
    │                                      │
    │   ┌──────────┐                       │
    │   │  client  │                       │
    │   └────┬─────┘                       │
    │        │                             │
    │        ↓                             │
    │   ┌──────────┐                       │
    │   │  server  │                       │
    │   └────┬─────┘                       │
    │        │ mongo:27017                 │
    │        ↓                             │
    │   ┌──────────┐                       │
    │   │  mongo   │                       │
    │   └──────────┘                       │
    │                                      │
    └──────────────────────────────────────┘

The service name acts as the hostname inside this network.

---

## 15. Key Learning

The most important Docker Compose networking concept learned here is:

    Container-to-container communication
    uses the Compose service name + container port.

For our application:

    server → mongo:27017

NOT:

    server → localhost:27017

This is one of the fundamental differences between running applications directly on the host and running them inside Docker Compose.



# Step 23 — Docker Compose

## Part 3 — Compose Lifecycle, Dependencies & Restart Management

---

## 1. `depends_on`

Docker Compose allows us to define service dependencies using:

    depends_on

Our project uses:

    client → server
    server → mongo

Meaning:

    client depends on server
    server depends on mongo

Compose configuration:

    client:
      depends_on:
        - server

    server:
      depends_on:
        - mongo

This establishes the startup dependency relationship between the services.

---

## 2. Dependency Flow

The application dependency structure is:

    mongo
      ↑
    server
      ↑
    client

Therefore:

    MongoDB
       ↓
    Backend
       ↓
    Frontend

The backend needs MongoDB, and the frontend depends on the backend.

---

## 3. `depends_on` Does Not Mean "Application Ready"

An important limitation:

    depends_on

controls the startup order/dependency relationship.

It does NOT automatically guarantee that the application inside the dependent service is fully ready to accept requests.

For example:

    server depends_on mongo

means Compose starts MongoDB before starting the server.

But MongoDB may still be initializing when the server starts.

Therefore application-level readiness may require:

    healthcheck

or application-level retry logic.

---

## 4. `restart: always`

Our Compose file uses:

    restart: always

for:

    client
    server
    mongo

This tells Docker to restart the container automatically when it stops.

Example:

    server:
      restart: always

This is useful for keeping application services running without manually starting them again.

---

## 5. Restart Policy Comparison

| Policy | Meaning |
|---|---|
| `no` | Do not automatically restart |
| `on-failure` | Restart when the container exits with failure |
| `always` | Always restart the container after it stops |
| `unless-stopped` | Restart unless explicitly stopped by the user |

For this learning project, we used:

    restart: always

---

## 6. Restarting a Single Service

A single Compose service can be restarted without restarting the entire stack.

Example:

    docker compose restart server

We used this during testing.

After restart, the server status returned to:

    Up

and its logs showed:

    app started at 5001...
    connected!

This confirmed that the server could restart successfully while remaining part of the Compose application.

---

## 7. Restarting the Entire Compose Stack

The entire stack can be restarted with:

    docker compose restart

This restarts the existing Compose containers.

We verified the containers after restart using:

    docker compose ps

All three services remained available:

    client
    server
    mongo

---

## 8. Stopping the Compose Stack

To stop and remove the Compose containers and network:

    docker compose down

During testing, this produced:

    Container ...client-1 Removed
    Container ...server-1 Removed
    Container ...mongo-1 Removed
    Network ..._default Removed

This demonstrates that Compose manages the lifecycle of the complete application stack.

---

## 9. `docker compose down` and Volumes

An important observation:

    docker compose down

removes the containers and Compose network.

It does NOT remove named volumes by default.

Our MongoDB data volume:

    blog-app-using-mern-stack_mongo-data

therefore remains available.

This is important because MongoDB data should survive container recreation.

---

## 10. Starting the Stack Again

After:

    docker compose down

we started the application again with:

    docker compose up -d

Compose recreated the required resources and started:

    mongo
    server
    client

The final status showed all three services as:

    Up

---

## 11. Compose Lifecycle Workflow

The complete lifecycle tested in this project was:

    docker compose up
           ↓
    Services running
           ↓
    docker compose ps
           ↓
    Test application
           ↓
    docker compose restart
           ↓
    Verify services
           ↓
    docker compose down
           ↓
    Containers removed
           ↓
    docker compose up -d
           ↓
    Stack recreated
           ↓
    Verify services again

---

## 12. Restart vs Down + Up

These commands are related but different.

| Command | What happens |
|---|---|
| `docker compose restart` | Restarts existing containers |
| `docker compose stop` | Stops containers |
| `docker compose start` | Starts existing stopped containers |
| `docker compose down` | Removes containers and Compose network |
| `docker compose up -d` | Creates/recreates and starts the stack |

Important difference:

    restart

does not recreate the Compose stack.

Whereas:

    down + up

removes and recreates the application resources managed by Compose.

---

## 13. Compose Configuration Warning

During our tests, Compose repeatedly displayed:

    the attribute `version` is obsolete, it will be ignored

Our file contains:

    version: "3.8"

Modern Docker Compose uses the Compose Specification and no longer requires this top-level `version` field.

The warning did not prevent the application from running.

The configuration was still successfully parsed and the services started correctly.

---

## 14. Current Compose Configuration

The application contains three services:

    client
    server
    mongo

And one named volume:

    mongo-data

Compose also automatically created the project network:

    blog-app-using-mern-stack_default

Therefore the overall structure is:

    Docker Compose Project
    │
    ├── client
    │     └── port 3000:80
    │
    ├── server
    │     └── port 5001:5001
    │
    ├── mongo
    │     └── port 27017:27017
    │
    ├── network
    │     └── blog-app-using-mern-stack_default
    │
    └── volume
          └── blog-app-using-mern-stack_mongo-data

---

## 15. Final Lifecycle Verification

After restarting and recreating the stack, we verified:

    docker compose ps

Expected services:

    client
    server
    mongo

Expected state:

    Up

Expected ports:

    client → 3000:80
    server → 5001:5001
    mongo → 27017:27017

The verification confirmed that the Compose stack could be:

    started
    restarted
    stopped
    removed
    recreated

without manually managing each container individually.

---

## 16. Key Learning

The main concepts learned in this part are:

    depends_on
    restart policies
    compose restart
    compose down
    compose up
    Compose lifecycle
    container recreation
    network lifecycle
    volume persistence

The key idea is:

    Docker Compose manages the lifecycle of the
    complete multi-container application as one project.



# Step 23 — Docker Compose

## Part 4 — Complete Workflow, Problems, Verification & Cheat Sheet

---

## 1. Complete Docker Compose Workflow

Project path:

    ~/Projects/mern-devops-practice/Blog-App-using-MERN-stack

Move to the project:

    cd ~/Projects/mern-devops-practice/Blog-App-using-MERN-stack

Main Compose file:

    docker-compose.yaml

The project contains three services:

    client
    server
    mongo

Overall architecture:

    Browser
       │
       ▼
    client :3000
       │
       ▼
    server :5001
       │
       ▼
    mongo :27017
       │
       ▼
    mongo-data volume

---

## 2. Compose Configuration Validation

Before starting the stack:

    docker compose config

Purpose:

    Parse and validate the Compose configuration.

It also shows the final configuration Docker Compose understands.

Useful for detecting:

    YAML errors
    indentation problems
    invalid configuration
    environment configuration problems
    service configuration problems

---

## 3. Start the Compose Stack

Start in detached mode:

    docker compose up -d

Important flag:

    -d

Meaning:

    Detached mode

The terminal returns immediately while containers continue running in the background.

Expected services:

    mongo
    server
    client

---

## 4. Check Compose Status

Command:

    docker compose ps

This shows:

    NAME
    IMAGE
    COMMAND
    SERVICE
    CREATED
    STATUS
    PORTS

Expected services:

    client
    mongo
    server

Expected ports:

    client → 3000:80
    server → 5001:5001
    mongo  → 27017:27017

---

## 5. Check All Docker Containers

Command:

    docker ps -a

Difference:

    docker compose ps

shows containers belonging to the current Compose project.

    docker ps -a

shows Docker containers globally, including containers from other projects.

---

## 6. View Compose Services

Command:

    docker compose config --services

Expected output:

    mongo
    server
    client

This is useful when you want to confirm which services are defined in the Compose configuration.

---

## 7. View Compose Logs

All services:

    docker compose logs

Follow logs continuously:

    docker compose logs -f

Specific service:

    docker compose logs server

Last 20 lines:

    docker compose logs --tail 20 server

Important flags:

    -f
    --tail

Meaning:

    -f
    Follow live logs.

    --tail 20
    Show only the last 20 log lines.

---

## 8. Execute Commands Inside a Compose Service

Example:

    docker compose exec server sh

Run a single command:

    docker compose exec server getent hosts mongo

Run MongoDB connectivity test:

    docker compose exec server sh -c 'nc -zv mongo 27017 2>&1 || busybox nc -zv mongo 27017 2>&1'

This verified:

    server → mongo DNS
    server → mongo TCP connectivity

---

## 9. Verify Service-to-Service DNS

Command:

    docker compose exec server getent hosts mongo

Expected result:

    172.x.x.x    mongo

This proves that the Compose network provides DNS resolution for the service name:

    mongo

Therefore the backend can use:

    mongodb://mongo:27017/BlogApp

instead of:

    mongodb://127.0.0.1:27017/BlogApp

Inside the server container:

    localhost

means the server container itself.

The MongoDB container must therefore be addressed by its Compose service name:

    mongo

---

## 10. Verify MongoDB TCP Connectivity

Command:

    docker compose exec server sh -c 'nc -zv mongo 27017 2>&1 || busybox nc -zv mongo 27017 2>&1'

Successful result:

    mongo (172.x.x.x:27017) open

This confirms:

    DNS works
    MongoDB is listening
    Docker network communication works
    server can reach MongoDB on port 27017

---

## 11. Verify Backend HTTP

Command:

    curl -i http://localhost:5001

Our backend returned:

    HTTP/1.1 404 Not Found

with:

    Cannot GET /

This was NOT a Docker failure.

The important observation was:

    HTTP response received
    Express responded
    backend process was running

The requested route `/` simply was not defined by the application.

Therefore:

    404 ≠ container failure

---

## 12. Verify Frontend HTTP

Command:

    curl -I http://localhost:3000

Expected result:

    HTTP/1.1 200 OK

This confirmed:

    frontend container is running
    port mapping works
    nginx is serving the frontend

---

# Problems Faced During Step 23

## 13. Problem — Obsolete `version` Warning

Repeated warning:

    the attribute `version` is obsolete, it will be ignored

Cause:

The Compose file contained:

    version: "3.8"

Modern Docker Compose no longer requires this top-level version field.

Effect:

    warning only

It did NOT prevent the stack from running.

Solution:

The warning can be removed by deleting:

    version: "3.8"

from the Compose file.

---

## 14. Problem — MongoDB URI Whitespace

Earlier the backend produced:

    MongoParseError:
    Invalid scheme

Expected:

    mongodb://

but the application was effectively receiving an invalid URI containing leading whitespace.

Cause:

The environment variable value had whitespace around the URI.

The MongoDB driver therefore could not recognize the connection string correctly.

Solution:

Verify the exact environment value and remove leading/trailing whitespace.

Correct value:

    mongodb://mongo:27017/BlogApp

Verification was performed by checking the URI inside the container.

---

## 15. Problem — Wrong MongoDB Host

A containerized backend must not use:

    mongodb://127.0.0.1:27017/...

for MongoDB running in another container.

Why?

Inside the backend container:

    127.0.0.1

refers to the backend container itself.

Correct Compose service-to-service address:

    mongodb://mongo:27017/BlogApp

because:

    mongo

is the Compose service name and Docker's internal DNS resolves it.

---

## 16. Problem — Old Standalone Containers

Before Compose was used, standalone containers existed:

    blog-backend-step22
    mongodb

These could cause confusion because the same application had both:

    standalone containers

and:

    Compose-managed containers

Solution:

Stop/remove the old standalone containers before starting the Compose stack.

Example:

    docker stop mongodb blog-backend-step22

    docker rm mongodb blog-backend-step22

Then start:

    docker compose up -d

This leaves Compose responsible for the application stack.

---

## 17. Problem — Backend `404 Not Found`

The backend returned:

    HTTP/1.1 404 Not Found

and:

    Cannot GET /

Cause:

The Express application did not define a GET route for `/`.

This does not mean:

    Docker failed
    backend failed to start
    MongoDB failed

Evidence:

Backend logs showed:

    app started at 5001...
    connected!

Therefore the container and backend process were functioning.

---

## 18. Problem — Persistence Test Initially Returned `null`

A persistence verification initially returned:

    null

The test was querying a specific database/collection/document.

Later the correct Compose volume persistence test created:

    test: "compose-volume"
    created: "step22.10"

Then MongoDB was restarted.

After restart, the same document was found again.

This confirmed that the named volume preserved MongoDB data.

---

## 19. Persistence Verification

Create test data:

    docker compose exec mongo mongosh \
      --quiet \
      --eval 'db = db.getSiblingDB("devops_test"); db.persistence.insertOne({test:"compose-volume", created:"step22.10"}); printjson(db.persistence.findOne({test:"compose-volume"}))'

Restart MongoDB:

    docker compose restart mongo

Verify again:

    docker compose exec mongo mongosh \
      --quiet \
      --eval 'db = db.getSiblingDB("devops_test"); printjson(db.persistence.findOne({test:"compose-volume"}))'

The document remained available.

Therefore:

    container restart
          ↓
    MongoDB data remains
          ↓
    named volume works

---

## 20. Problem — PC Restart

The PC was powered off during the learning process.

After starting Ubuntu again, the Compose project was checked using:

    cd ~/Projects/mern-devops-practice/Blog-App-using-MERN-stack

    docker compose ps

The Compose containers were still present and running.

The important lesson:

Docker resources are not the same as the terminal session.

The terminal closing or PC restarting does not mean the Docker Compose configuration disappears.

---

# Important Commands

## 21. Compose Command Cheat Sheet

| Command | Purpose |
|---|---|
| `docker compose config` | Validate/render Compose configuration |
| `docker compose config --services` | List services |
| `docker compose up -d` | Create/start stack in background |
| `docker compose ps` | Show Compose service status |
| `docker compose logs` | Show logs |
| `docker compose logs -f` | Follow live logs |
| `docker compose logs --tail 20 server` | Show last 20 server log lines |
| `docker compose exec server sh` | Open shell inside server |
| `docker compose exec server <command>` | Run command inside server |
| `docker compose restart` | Restart entire Compose stack |
| `docker compose restart server` | Restart only server |
| `docker compose stop` | Stop Compose services |
| `docker compose start` | Start stopped Compose services |
| `docker compose down` | Stop/remove containers and Compose network |
| `docker compose up -d` | Recreate/start stack when needed |

---

## 22. Important Flags

| Flag | Meaning | Example |
|---|---|---|
| `-d` | Detached/background mode | `docker compose up -d` |
| `-f` | Follow logs | `docker compose logs -f` |
| `--tail` | Limit log output | `docker compose logs --tail 20 server` |
| `--services` | Show service names | `docker compose config --services` |
| `--quiet` / `-q` | Reduce command output where supported | `mongosh --quiet` |

---

# Final Verification

## 23. Final Compose Verification Workflow

From the project directory:

    cd ~/Projects/mern-devops-practice/Blog-App-using-MERN-stack

Check configuration:

    docker compose config

Check services:

    docker compose config --services

Start stack:

    docker compose up -d

Check status:

    docker compose ps

Check frontend:

    curl -I http://localhost:3000

Check backend:

    curl -i http://localhost:5001

Check backend logs:

    docker compose logs --tail 20 server

Check server → MongoDB DNS:

    docker compose exec server getent hosts mongo

Check server → MongoDB port:

    docker compose exec server sh -c 'nc -zv mongo 27017 2>&1 || busybox nc -zv mongo 27017 2>&1'

Check volume:

    docker volume ls | grep blog-app-using-mern-stack

Final status:

    docker compose ps

---

## 24. Expected Final State

Services:

    client   → Up
    server   → Up
    mongo    → Up

Ports:

    client   → 3000:80
    server   → 5001:5001
    mongo    → 27017:27017

Network:

    blog-app-using-mern-stack_default

Volume:

    blog-app-using-mern-stack_mongo-data

Backend:

    app started at 5001...
    connected!

Frontend:

    HTTP 200 OK

Backend:

    HTTP response received
    `/` returns 404 because no root route is defined

Server → MongoDB:

    DNS resolves `mongo`
    TCP 27017 is open

MongoDB:

    Data survives container restart through named volume.

---

# Step 23 Final Concept

Docker Compose changed the workflow from:

    manually managing containers
    manually creating networks
    manually configuring environment variables
    manually connecting services
    manually restarting containers

to:

    docker-compose.yaml
           ↓
    docker compose up
           ↓
    client + server + mongo
           ↓
    automatic Compose network
           ↓
    service-name DNS
           ↓
    named volume
           ↓
    unified lifecycle management

The main learning:

    Docker manages containers.

    Docker Compose manages the multi-container application.

---

# Step 23 — Completion Checklist

- [x] Compose file structure understood
- [x] Services defined
- [x] Ports configured
- [x] Environment variable configured
- [x] Compose network verified
- [x] Service-name DNS verified
- [x] Server → MongoDB connectivity verified
- [x] `depends_on` understood
- [x] Restart policy understood
- [x] `docker compose up` tested
- [x] `docker compose restart` tested
- [x] `docker compose down` tested
- [x] Stack recreated successfully
- [x] Named volume verified
- [x] MongoDB persistence verified
- [x] Frontend HTTP verified
- [x] Backend HTTP verified
- [x] Backend logs verified
- [x] Problems and fixes documented
- [x] Important commands and flags documented
- [x] Final Compose stack verified


