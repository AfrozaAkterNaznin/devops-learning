# Step 19 — Redis

## Part 1 — Concepts & Architecture

---

## 1. Objective

The objective of Step 19 was to understand Redis as an in-memory data store and introduce Redis into the existing Blog-App-using-MERN-stack development environment.

The practical work focused on:

- Redis environment verification
- Redis service management
- Redis CLI usage
- Redis connectivity
- Basic Redis operations
- Redis data types
- Node.js and Redis integration
- Redis caching
- Redis TTL
- Redis persistence and configuration
- Redis operational monitoring
- Final Redis project verification

---

## 2. Learning Outcomes

After completing Step 19, the following concepts were practically verified:

- Redis installation and environment verification
- Redis CLI usage
- Redis server connectivity
- Redis `SET`, `GET`, `EXISTS`, and `DEL`
- Redis String, List, Set, and Hash data types
- Redis TTL
- Cache HIT and Cache MISS
- Node.js to Redis connectivity
- Redis persistence concepts
- RDB persistence
- AOF persistence
- Redis memory and client information
- Redis service logs
- Redis health verification
- Redis and MongoDB running together in the project environment

---

## 3. Workflow

The complete practical workflow followed in Step 19 was:

    Redis Environment
           |
           v
    Redis Connectivity
           |
           v
    Basic Redis Operations
           |
           v
    Redis Data Types
           |
           v
    Node.js Redis Integration
           |
           v
    Redis Cache
           |
           v
    Persistence & Configuration
           |
           v
    Troubleshooting
           |
           v
    Final Project Verification

---

# 4. Theory

## 4.1 What is Redis?

Redis is an in-memory data store commonly used for:

- Caching
- Session storage
- Temporary data
- Counters
- Queues
- Fast key-value operations

Redis keeps frequently accessed data in memory, which allows very fast read and write operations.

Unlike MongoDB, which is primarily used as the application's persistent database, Redis is commonly used for temporary, frequently accessed, or performance-sensitive data.

---

## 4.2 Redis in This Project

The existing Blog-App-using-MERN-stack project uses MongoDB as its main application database.

Redis was introduced as an additional backend service for learning and caching purposes.

The environment can be represented as:

    Blog Application
            |
            v
      Node.js / Express
         /          \
        /            \
       v              v
    MongoDB          Redis
     :27017          :6379
    Persistent     Cache / Fast
       Data            Data

MongoDB remains responsible for persistent application data.

Redis provides a separate in-memory data store that can be used for caching and other fast-access operations.

---

# 5. Architecture Overview

## 5.1 Redis Runtime

Redis was running as a native Ubuntu systemd service.

    Ubuntu
       |
       v
    systemd
       |
       v
    redis-server.service
       |
       v
    Redis 8.0.5
       |
       v
    127.0.0.1:6379

The Redis service was verified as:

    redis-server.service
    Active: active (running)

Redis was listening on:

    127.0.0.1:6379
    [::1]:6379

---

## 5.2 Redis and Docker

Redis was NOT running as a Docker container during this step.

The MongoDB database was running through the existing Docker container:

    MongoDB
       |
       └── Docker Container
           Name: mongodb
           Image: mongo:8
           Port: 27017

Redis was running directly through Ubuntu:

    Redis
       |
       └── systemd service
           Name: redis-server.service
           Port: 6379

This distinction was verified during the practical work.

---

# 6. Redis Role in the Project

Redis was introduced as a supporting service rather than replacing MongoDB.

    MongoDB
       |
       └── Persistent Application Data

    Redis
       |
       └── Cache / Fast Temporary Data

A typical cache workflow is:

    Client Request
          |
          v
       Node.js
          |
          v
        Redis
          |
          +---- Cache HIT ----> Return cached data
          |
          +---- Cache MISS ---> Read database
                                  |
                                  v
                             Store in Redis
                                  |
                                  v
                               Response

The practical work verified Node.js-to-Redis connectivity and cache behavior.

A complete production caching layer was not added to the Blog API source code during this step.

---

# 7. Important Redis Concepts

| Concept | Meaning |
|---|---|
| Redis | In-memory data store |
| Key | Identifier used to access stored data |
| Value | Data associated with a key |
| String | Single value |
| List | Ordered collection of values |
| Set | Collection of unique values |
| Hash | Field-value collection |
| TTL | Time To Live |
| Cache HIT | Requested key exists in Redis |
| Cache MISS | Requested key does not exist |
| RDB | Snapshot-based Redis persistence |
| AOF | Append Only File persistence |
| `redis-cli` | Redis command-line client |
| Port `6379` | Default Redis port |

---

# 8. Redis Data Types

The following Redis data types were practically tested:

| Data Type | Practical Data | Characteristic |
|---|---|---|
| String | `MERN` | Single value |
| List | `Node.js`, `Docker`, `Redis` | Ordered collection |
| Set | `MongoDB`, `PostgreSQL`, `Redis` | Unique values |
| Hash | `name`, `role`, `stack` | Field-value pairs |

The practical test confirmed the following Redis key types:

    string
    list
    set
    hash

The test data was cleaned up after verification.

---

# 9. Cache HIT and Cache MISS

Redis caching was practically tested in Step 19.

## Cache MISS

When the requested key does not exist:

    (nil)

This represents a Cache MISS.

The application can then retrieve the required data from the database and store the result in Redis.

## Cache HIT

When the requested key already exists:

    "BlogApp-Data"

Redis immediately returns the cached value.

This represents a Cache HIT.

---

## Cache Flow

    Request
       |
       v
    Check Redis
       |
       +---- Key exists ----> Cache HIT
       |                         |
       |                         v
       |                    Return value
       |
       +---- Key missing ---> Cache MISS
                                 |
                                 v
                           Get database data
                                 |
                                 v
                           Store in Redis
                                 |
                                 v
                              Response

---

# 10. TTL — Time To Live

TTL determines how long a Redis key remains available.

During the practical cache test:

    TTL = 60 seconds

The workflow was:

    Store Cache
        |
        v
    Set TTL = 60 seconds
        |
        v
    Cache available
        |
        v
    Automatically expires after TTL

TTL is useful for temporary cached data because stale data can automatically expire.

---

# 11. Redis Persistence

Redis is primarily an in-memory data store, but it also provides persistence mechanisms.

Two important persistence mechanisms are:

    RDB
    AOF

---

## 11.1 RDB

RDB is Redis's snapshot-based persistence mechanism.

It saves Redis data as snapshots at configured intervals.

The observed configuration was:

    save
    3600 1 300 100 60 10000

The practical environment also reported:

    rdb_last_bgsave_status: ok

This indicates that the last background RDB save completed successfully.

---

## 11.2 AOF

AOF stands for Append Only File.

Instead of periodically saving snapshots, AOF records write operations so Redis can reconstruct the dataset.

The observed configuration was:

    appendonly
    no

The persistence status also reported:

    aof_enabled: 0

Therefore:

    AOF persistence: Disabled

---

# 12. RDB vs AOF

| Feature | RDB | AOF |
|---|---|---|
| Full Name | Redis snapshot persistence | Append Only File |
| Method | Periodic snapshots | Records write operations |
| Persistence Type | Snapshot | Operation log |
| Observed Status | Configured | Disabled |
| Practical Observation | `rdb_last_bgsave_status: ok` | `aof_enabled: 0` |

---

# 13. Redis Environment

The actual Redis environment verified during Step 19:

| Configuration | Actual Value |
|---|---|
| Redis Version | `8.0.5` |
| Redis CLI | `8.0.5` |
| Redis Service | `redis-server.service` |
| Service Status | `active (running)` |
| Service Enabled | `enabled` |
| Redis Port | `6379` |
| Bind Address | `127.0.0.1` |
| IPv6 Address | `::1` |
| Data Directory | `/var/lib/redis` |
| Database Count | `16` |
| AOF | Disabled |
| RDB | Configured |
| Last RDB Background Save | `ok` |
| Maximum Memory | `0B` |
| Current Used Memory | Approximately `1.10M` |

---

# 14. Important Project Files

The Redis integration work affected the Node.js backend environment.

Important project files:

| File | Purpose |
|---|---|
| `server/package.json` | Node.js dependencies and scripts |
| `server/package-lock.json` | npm dependency lock file |
| `server/yarn.lock` | Existing Yarn dependency lock file |
| `server/.env` | Environment configuration |
| `server/.env.example` | Environment configuration example |

The Redis Node.js client was added to the backend:

    redis@6.2.1

---

# 15. Important Directories

| Directory | Purpose |
|---|---|
| `~/Projects/mern-devops-practice/Blog-App-using-MERN-stack` | Main MERN project |
| `~/Projects/mern-devops-practice/Blog-App-using-MERN-stack/server` | Node.js / Express backend |
| `/var/lib/redis` | Redis data directory |

---

# 16. Redis vs MongoDB

| Feature | Redis | MongoDB |
|---|---|---|
| Main Role | Cache / fast data store | Application database |
| Data Model | Key-value / multiple data types | Document database |
| Primary Storage | In-memory | Persistent database storage |
| Default Port | `6379` | `27017` |
| Runtime in Project | Native systemd service | Docker container |
| Service / Container | `redis-server.service` | `mongodb` |
| Version | `8.0.5` | `8.2.12` |
| Main Use in Project | Cache / fast temporary data | Blog application data |

---

# 17. Node.js Redis Integration

The existing backend initially had no Redis dependency.

Before integration:

    redis dependency
    (empty)

The Redis Node.js client was then installed.

After installation:

    redis@6.2.1

The Node.js connection test successfully produced:

    Redis connection: OK
    Redis value: Node.js → Redis
    Redis test key deleted
    Redis connection closed

This confirmed:

    Node.js
        |
        v
      Redis

connectivity.

---

# 18. Redis Operational Environment

Redis operational information was also inspected.

Observed values included:

    redis_version:8.0.5
    process_id:2083
    tcp_port:6379
    uptime_in_seconds:14861
    connected_clients:1
    blocked_clients:0
    used_memory_human:1.10M
    maxmemory_human:0B
    total_connections_received:39
    total_commands_processed:44
    instantaneous_ops_per_sec:0

Redis service logs were also checked using:

    journalctl -u redis-server

The logs confirmed successful Redis service starts.

---

# 19. Summary

Step 19 established Redis as an additional backend service alongside the existing MongoDB environment.

The complete architecture is:

    MERN Blog Application
            |
            v
     Node.js / Express
        /           \
       /             \
      v               v
   MongoDB           Redis
    :27017            :6379
      |                 |
      v                 v
 Persistent Data   Cache / Fast Data

The Redis environment was successfully verified from environment setup through final project verification.

Key final values:

    Redis Version        : 8.0.5
    Redis Port           : 6379
    Redis Service        : redis-server.service
    Redis Status         : active
    Redis Data Directory : /var/lib/redis
    Node.js Redis Client  : redis@6.2.1
    AOF                  : Disabled
    RDB                  : Configured
    MongoDB              : Docker container
    MongoDB Database     : BlogApp

## Part 1 Conclusion

Step 19 successfully established the Redis foundation required for the practical DevOps workflow:

    Environment
       ↓
    Connectivity
       ↓
    Data Types
       ↓
    Node.js Integration
       ↓
    Caching
       ↓
    Persistence
       ↓
    Troubleshooting
       ↓
    Final Verification

## Part 2 — Commands & Operations

---

## 1. Redis Environment Commands

### 1.1 Verify Redis CLI

    which redis-cli

Purpose:

Verifies the location of the Redis command-line client.

Observed:

    /usr/bin/redis-cli

---

### 1.2 Verify Redis CLI Version

    redis-cli --version

Purpose:

Checks the installed Redis CLI version.

Observed:

    redis-cli 8.0.5

---

### 1.3 Check Redis Service Status

    systemctl status redis-server

Purpose:

Checks whether the Redis systemd service is running.

Important information:

- Service name
- Active state
- Main PID
- Memory usage
- Redis process
- Service logs

Observed:

    Active: active (running)

---

### 1.4 Check Redis Service Enablement

    systemctl is-enabled redis-server

Purpose:

Checks whether Redis is configured to start automatically during system boot.

Observed:

    enabled

---

### 1.5 Check Redis Listening Port

    ss -lntp | grep 6379

Purpose:

Verifies whether Redis is listening on its default TCP port.

Observed:

    127.0.0.1:6379
    [::1]:6379

---

## 2. Redis Connectivity Commands

### 2.1 Test Redis Connection

    redis-cli ping

Purpose:

Checks whether the Redis server is responding.

Expected result:

    PONG

Observed:

    PONG

---

### 2.2 Check Redis Server Information

    redis-cli INFO server | grep -E 'redis_version|uptime_in_seconds|tcp_port|process_id'

Purpose:

Displays important Redis server information.

The command filters the output to show:

- Redis version
- Server uptime
- TCP port
- Redis process ID

Observed:

    redis_version:8.0.5
    process_id:2083
    tcp_port:6379

---

## 3. Basic Redis Operations

Redis stores data using key-value operations.

### 3.1 SET

    redis-cli SET step19:test "Redis-Step-19"

Purpose:

Creates a Redis key and stores a value.

Expected result:

    OK

---

### 3.2 GET

    redis-cli GET step19:test

Purpose:

Retrieves the value stored under a Redis key.

Example result:

    "Redis-Step-19"

---

### 3.3 EXISTS

    redis-cli EXISTS step19:test

Purpose:

Checks whether a key exists.

Result:

    1

Meaning:

    1 = key exists
    0 = key does not exist

---

### 3.4 DELETE

    redis-cli DEL step19:test

Purpose:

Deletes the specified Redis key.

Expected result:

    (integer) 1

---

### 3.5 Verify Deleted Key

    redis-cli EXISTS step19:test

Expected result:

    (integer) 0

This confirms that the key was successfully removed.

---

## 4. Redis Data Types

The following Redis data types were practically tested:

- String
- List
- Set
- Hash

---

### 4.1 String

Example:

    SET step19:string "MERN"

Retrieve:

    GET step19:string

Observed:

    "MERN"

A String stores a single value associated with a key.

---

### 4.2 List

Example:

    RPUSH step19:list "Node.js" "Docker" "Redis"

Read the list:

    LRANGE step19:list 0 -1

Observed values:

    Node.js
    Docker
    Redis

A List is an ordered collection of values.

---

### 4.3 Set

Example:

    SADD step19:set "MongoDB" "PostgreSQL" "Redis"

Read the Set:

    SMEMBERS step19:set

Observed values:

    MongoDB
    PostgreSQL
    Redis

A Set stores unique values.

---

### 4.4 Hash

Example:

    HSET step19:user name "Afroza" role "DevOps Learner" stack "MERN"

Read the Hash:

    HGETALL step19:user

Observed fields and values:

    name
    Afroza
    role
    DevOps Learner
    stack
    MERN

A Hash stores field-value pairs under a single key.

---

### 4.5 Check Redis Key Types

    TYPE step19:string
    TYPE step19:list
    TYPE step19:set
    TYPE step19:user

Observed key types:

    string
    list
    set
    hash

---

### 4.6 Cleanup Test Data

The Redis data-type practice keys were removed after verification.

Cleanup example:

    DEL step19:string step19:list step19:set step19:user

Purpose:

Prevents temporary learning data from remaining in Redis.

---

# 5. Node.js Redis Integration

## 5.1 Navigate to Backend

    cd ~/Projects/mern-devops-practice/Blog-App-using-MERN-stack/server

Purpose:

Moves into the Node.js backend directory where Redis integration was performed.

---

## 5.2 Check Existing Redis Dependency

    npm list redis --depth=0

Before Redis integration, the project did not have the Redis package installed.

Observed:

    (empty)

---

## 5.3 Install Redis Node.js Client

    npm install redis

Purpose:

Installs the official Node.js Redis client package.

Observed dependency:

    redis@6.2.1

The installation modified the Node.js dependency files.

Updated project files included:

    package.json
    package-lock.json

The project also showed an existing modification to:

    yarn.lock

---

## 5.4 Verify Redis Dependency

    npm list redis --depth=0

Observed:

    blogapp@1.0.0
    └── redis@6.2.1

This confirms that the Node.js backend has the Redis client dependency.

---

## 5.5 Node.js Redis Connection Test

The Node.js Redis client was used to establish a connection to the Redis server.

The connection test verified:

    Redis connection: OK

The test also verified:

    Redis value: Node.js → Redis

The temporary Redis key was then deleted and the connection was closed.

Observed:

    Redis connection: OK
    Redis value: Node.js → Redis
    Redis test key deleted
    Redis connection closed

This confirms successful:

    Node.js → Redis

communication.

---

# 6. Redis Cache Operations

Redis caching was practically tested using a temporary cache key.

---

## 6.1 Clear Previous Cache

    redis-cli DEL step19:cache

Purpose:

Removes an existing test cache key before starting the cache test.

---

## 6.2 First Request — Cache MISS

    redis-cli GET step19:cache

Observed:

    (nil)

Meaning:

The requested cache key did not exist.

Therefore:

    Cache MISS

---

## 6.3 Store Database Result in Cache

    redis-cli SETEX step19:cache 60 "BlogApp-Data"

Purpose:

Stores a value in Redis with a 60-second expiration time.

The cache value represents data that could have been retrieved from the application's database.

Observed:

    OK

---

## 6.4 Second Request — Cache HIT

    redis-cli GET step19:cache

Observed:

    "BlogApp-Data"

Meaning:

The requested value was already available in Redis.

Therefore:

    Cache HIT

---

## 6.5 Check Cache TTL

    redis-cli TTL step19:cache

Observed:

    (integer) 60

The TTL represents the remaining lifetime of the cache key in seconds.

---

## 6.6 Cleanup Cache

    redis-cli DEL step19:cache

Purpose:

Removes the temporary cache key after testing.

Observed:

    (integer) 1

---

# 7. Redis Persistence Commands

## 7.1 Check Persistence Configuration

    redis-cli CONFIG GET save

Purpose:

Checks the configured RDB snapshot rules.

Observed:

    save
    3600 1 300 100 60 10000

---

## 7.2 Check AOF Configuration

    redis-cli CONFIG GET appendonly

Purpose:

Checks whether Append Only File persistence is enabled.

Observed:

    appendonly
    no

Therefore:

    AOF = Disabled

---

## 7.3 Check Redis Data Directory

    redis-cli CONFIG GET dir

Purpose:

Checks where Redis stores its persistent data files.

Observed:

    /var/lib/redis

---

## 7.4 Check Number of Redis Databases

    redis-cli CONFIG GET databases

Purpose:

Checks how many logical Redis databases are configured.

Observed:

    16

---

## 7.5 Check Persistence Status

    redis-cli INFO persistence

Important values observed:

    loading:0
    async_loading:0
    rdb_changes_since_last_save:4
    rdb_last_bgsave_status:ok
    aof_enabled:0
    aof_rewrite_in_progress:0

Important observations:

    rdb_last_bgsave_status:ok

means the last background RDB save was successful.

    aof_enabled:0

means AOF persistence is disabled.

---

# 8. Redis Memory Inspection

## 8.1 Check Redis Memory

    redis-cli INFO memory | grep -E 'used_memory_human|maxmemory_human'

Observed:

    used_memory_human:1.10M
    maxmemory_human:0B

`used_memory_human` shows the memory currently used by Redis.

`maxmemory_human:0B` means no explicit Redis maxmemory limit was configured.

---

# 9. Redis Client and Statistics Commands

## 9.1 Check Connected Clients

    redis-cli INFO clients | grep -E 'connected_clients|blocked_clients'

Observed:

    connected_clients:1
    blocked_clients:0

Important:

    connected_clients

Shows the number of currently connected Redis clients.

    blocked_clients

Shows clients currently waiting on blocking operations.

---

## 9.2 Check Redis Statistics

    redis-cli INFO stats | grep -E 'total_connections_received|total_commands_processed|instantaneous_ops_per_sec'

Observed:

    total_connections_received:39
    total_commands_processed:44
    instantaneous_ops_per_sec:0

These values provide a basic operational view of Redis activity.

---

# 10. Redis Service Logs

## 10.1 View Redis Logs

    journalctl -u redis-server --no-pager -n 20

Purpose:

Displays the latest Redis service log entries without opening the interactive pager.

The logs showed successful Redis service startup events.

Example:

    Starting redis-server.service
    Started redis-server.service

---

## 10.2 Check Redis Service Again

    systemctl status redis-server

Purpose:

Confirms that Redis remains operational after configuration and testing.

Observed:

    Active: active (running)

---

# 11. Final Redis Verification Commands

The final verification checked Redis service, connectivity, version, port, Node.js dependency, Node.js connection, cache behavior, and the existing MongoDB container.

---

## 11.1 Redis Service

    systemctl is-active redis-server
    systemctl is-enabled redis-server

Observed:

    active
    enabled

---

## 11.2 Redis Health

    redis-cli ping

Observed:

    PONG

---

## 11.3 Redis Version

    redis-cli --version

Observed:

    redis-cli 8.0.5

---

## 11.4 Redis Port

    ss -lntp | grep 6379

Observed:

    127.0.0.1:6379
    [::1]:6379

---

## 11.5 Node.js Redis Dependency

    cd ~/Projects/mern-devops-practice/Blog-App-using-MERN-stack/server
    npm list redis --depth=0

Observed:

    redis@6.2.1

---

## 11.6 Node.js Redis Connection

The Node.js integration test confirmed:

    Node.js → Redis: Redis-Step-19

This verified that the Node.js backend can communicate with Redis.

---

## 11.7 Final Cache Verification

The final cache test confirmed:

    OK
    "BlogApp-Cache"
    (integer) 60
    (integer) 1

Meaning:

- Cache value was stored successfully
- Cache value was retrieved successfully
- TTL was configured
- Cache key was deleted successfully

---

## 11.8 Verify Existing MongoDB Container

    docker ps --filter "name=mongodb"

Observed:

    mongodb
    mongo:8
    0.0.0.0:27017->27017/tcp

This confirmed that Redis and the existing MongoDB environment were both available.

---

# 12. Redis CLI vs Linux Shell

One important issue encountered during the practical work was the difference between the Redis CLI and the normal Linux shell.

When inside Redis CLI:

    127.0.0.1:6379>

Redis commands such as:

    SET
    GET
    EXISTS
    DEL
    TYPE
    HSET
    SADD
    RPUSH

are interpreted by Redis.

After returning to the Linux shell:

    afroza@afroza-VirtualBox:~$

those Redis commands are no longer Linux commands.

For example:

    show collections

is a MongoDB shell command and should not be executed directly in the Linux shell.

The same principle applies to Redis commands.

To run Redis commands from the Linux shell, use:

    redis-cli <command>

---

# 13. Common Redis Mistakes

### Mistake 1 — Redis service is not running

Check:

    systemctl status redis-server

Start:

    sudo systemctl start redis-server

---

### Mistake 2 — Redis does not respond

Test:

    redis-cli ping

Expected:

    PONG

---

### Mistake 3 — Wrong Redis port

Check:

    ss -lntp | grep 6379

Default Redis port:

    6379

---

### Mistake 4 — Cache key does not exist

Check:

    redis-cli EXISTS <key>

Retrieve:

    redis-cli GET <key>

If the result is:

    (nil)

the key does not currently exist.

---

### Mistake 5 — Cache expired

Check:

    redis-cli TTL <key>

If the key has expired, Redis will no longer return its value.

---

### Mistake 6 — AOF assumed to be enabled

Check:

    redis-cli CONFIG GET appendonly

Observed in this environment:

    appendonly
    no

Therefore AOF is disabled.

---

# 14. Native Redis Service vs Docker Redis

In this project environment, Redis is running natively.

Current Redis runtime:

    redis-server.service
    Port: 6379

MongoDB is running in Docker:

    Container: mongodb
    Image: mongo:8
    Port: 27017

Therefore:

    Redis  → Native Ubuntu / systemd
    MongoDB → Docker

This distinction is important when troubleshooting service status, networking, ports, volumes, and logs.

---

# 15. Important Redis Commands — Quick Reference

| Command | Purpose |
|---|---|
| `redis-cli ping` | Test Redis connectivity |
| `redis-cli --version` | Check Redis CLI version |
| `redis-cli SET key value` | Store a value |
| `redis-cli GET key` | Retrieve a value |
| `redis-cli EXISTS key` | Check key existence |
| `redis-cli DEL key` | Delete a key |
| `redis-cli TYPE key` | Check key type |
| `redis-cli TTL key` | Check remaining TTL |
| `redis-cli SETEX key seconds value` | Store value with expiration |
| `redis-cli INFO server` | Server information |
| `redis-cli INFO memory` | Memory information |
| `redis-cli INFO clients` | Client information |
| `redis-cli INFO stats` | Redis statistics |
| `redis-cli INFO persistence` | Persistence status |
| `redis-cli CONFIG GET save` | RDB configuration |
| `redis-cli CONFIG GET appendonly` | AOF configuration |
| `redis-cli CONFIG GET dir` | Redis data directory |
| `redis-cli CONFIG GET databases` | Number of databases |
| `systemctl status redis-server` | Redis service status |
| `systemctl is-active redis-server` | Active state |
| `systemctl is-enabled redis-server` | Boot enablement |
| `ss -lntp \| grep 6379` | Redis listening port |
| `journalctl -u redis-server` | Redis service logs |
| `npm list redis --depth=0` | Node.js Redis dependency |

---

# 16. Commands and Observed Results Summary

| Area | Command / Tool | Observed Result |
|---|---|---|
| Redis CLI | `redis-cli --version` | `8.0.5` |
| Service | `systemctl status redis-server` | Active |
| Enablement | `systemctl is-enabled redis-server` | Enabled |
| Connectivity | `redis-cli ping` | `PONG` |
| Port | `ss -lntp` | `6379` |
| String | `SET / GET` | Successful |
| List | `RPUSH / LRANGE` | Successful |
| Set | `SADD / SMEMBERS` | Successful |
| Hash | `HSET / HGETALL` | Successful |
| Key Type | `TYPE` | String/List/Set/Hash |
| Cache MISS | `GET` | `(nil)` |
| Cache HIT | `GET` | `"BlogApp-Data"` |
| TTL | `TTL` | `60` seconds |
| Node.js | `npm list redis` | `redis@6.2.1` |
| Node.js Connection | Redis test | Successful |
| RDB | `INFO persistence` | Background save OK |
| AOF | `CONFIG GET appendonly` | Disabled |
| Data Directory | `CONFIG GET dir` | `/var/lib/redis` |
| Memory | `INFO memory` | `1.10M` used |
| Clients | `INFO clients` | `1` connected |
| Logs | `journalctl` | Service starts verified |
| MongoDB | `docker ps` | `mongodb` running |

---

# 17. Part 2 Summary

Step 19 practical commands covered the complete Redis operational workflow:

    Environment
       ↓
    Service Verification
       ↓
    Connectivity
       ↓
    Basic Operations
       ↓
    Data Types
       ↓
    Node.js Integration
       ↓
    Cache HIT / MISS
       ↓
    TTL
       ↓
    Persistence
       ↓
    Memory & Statistics
       ↓
    Troubleshooting
       ↓
    Final Verification

The commands were executed against the actual Redis environment used during the project.

## Part 2 Conclusion

The practical command work confirmed that Redis is:

- Installed
- Running
- Enabled
- Listening on port `6379`
- Accessible through `redis-cli`
- Accessible from Node.js
- Capable of storing multiple Redis data types
- Working as a cache
- Supporting TTL
- Configured with RDB persistence
- Running with AOF disabled
- Operational alongside the project's MongoDB container


## Part 3 — Actual Lab Record

---

## 1. Skills Gained

Step 19 practical work-এর মাধ্যমে নিম্নলিখিত skills অর্জন করা হয়েছে:

- Redis installation and environment verification
- Redis systemd service management
- Redis CLI usage
- Redis connectivity testing
- Redis key-value operations
- Redis String, List, Set and Hash data types
- Redis key existence and deletion
- Redis TTL and cache expiration
- Node.js and Redis integration
- Redis caching workflow
- Redis persistence concepts
- RDB configuration inspection
- AOF configuration inspection
- Redis memory monitoring
- Redis client monitoring
- Redis statistics inspection
- Redis service log analysis
- Redis troubleshooting
- Final Redis project verification
- Redis and MongoDB together in the same project environment

---

# 2. Actual Lab Environment

The Redis practical work was performed on the Ubuntu VirtualBox environment.

### Operating Environment

    Ubuntu Linux
    VirtualBox

### Redis Runtime

    Native Ubuntu Service

Redis was not running as a Docker container.

The Redis service was managed through systemd:

    redis-server.service

---

# 3. Actual Redis Version

The installed Redis CLI version was verified as:

    redis-cli 8.0.5

The Redis server information also reported:

    redis_version:8.0.5

Therefore, the practical environment used:

    Redis Server: 8.0.5
    Redis CLI: 8.0.5

---

# 4. Actual Redis Service

Redis was running as a native Linux system service.

Observed service:

    redis-server.service

Service state:

    active (running)

Boot enablement:

    enabled

This means Redis was:

- Currently running
- Configured to start automatically during system boot

The Redis process was observed as:

    /usr/bin/redis-server 127.0.0.1:6379

---

# 5. Actual Redis Port

Redis was listening on the default port:

    6379

Observed listeners:

    127.0.0.1:6379
    [::1]:6379

Therefore Redis was available locally through port:

    6379

The service was bound to localhost rather than being exposed directly to the external network.

---

# 6. Actual Redis Connectivity Verification

The Redis server was tested using:

    redis-cli ping

Observed result:

    PONG

This confirmed that the Redis server was responding correctly.

The Redis server information also showed:

    tcp_port:6379

Therefore both service availability and network listening were successfully verified.

---

# 7. Actual Redis Data Type Testing

Four Redis data types were practically tested.

### String

Stored value:

    MERN

Observed type:

    string

### List

Stored values:

    Node.js
    Docker
    Redis

Observed type:

    list

### Set

Stored values:

    MongoDB
    PostgreSQL
    Redis

Observed type:

    set

### Hash

Stored fields:

    name = Afroza
    role = DevOps Learner
    stack = MERN

Observed type:

    hash

The `TYPE` command confirmed:

    string
    list
    set
    hash

All temporary test data was removed after verification.

---

# 8. Actual Redis CRUD-Style Operations

The practical work verified the basic Redis key-value workflow.

### Create / Store

    SET

### Read

    GET

### Check Existence

    EXISTS

### Delete

    DEL

A temporary key was successfully:

    Created
    Retrieved
    Checked
    Deleted

After deletion:

    EXISTS = 0

This confirmed successful basic Redis key management.

---

# 9. Actual Redis Cache Test

Redis was practically tested as a caching layer.

### First Request

The cache key did not exist.

Observed:

    (nil)

Therefore:

    Cache MISS

### Store Cache

The database-style result was stored in Redis:

    BlogApp-Data

### Second Request

The same key was requested again.

Observed:

    "BlogApp-Data"

Therefore:

    Cache HIT

### Cache Expiration

The cache TTL was verified as:

    60 seconds

This demonstrated the basic caching workflow:

    Request
       ↓
    Cache MISS
       ↓
    Store Data in Redis
       ↓
    Request Again
       ↓
    Cache HIT
       ↓
    Data Returned from Redis

---

# 10. Actual Node.js Redis Integration

Redis was integrated with the project's Node.js backend.

Project:

    Blog-App-using-MERN-stack

Backend directory:

    server

Before integration, the project did not contain the Redis Node.js dependency.

After installation:

    redis@6.2.1

was present in the backend.

Therefore the actual environment contains:

    Redis Server: 8.0.5
    Node.js Redis Client: 6.2.1

---

# 11. Actual Node.js Connection Test

The Node.js backend successfully connected to Redis.

Observed:

    Redis connection: OK

The Node.js application also successfully stored and retrieved a Redis value.

Observed:

    Redis value: Node.js → Redis

The test key was then deleted and the Redis connection was closed.

Final observed result:

    Redis connection: OK
    Redis value: Node.js → Redis
    Redis test key deleted
    Redis connection closed

Therefore:

    Node.js → Redis

communication was successfully verified.

---

# 12. Actual Redis Persistence Configuration

Redis persistence configuration was inspected.

### RDB

Observed configuration:

    save
    3600 1 300 100 60 10000

Therefore RDB snapshot rules were configured.

The persistence status reported:

    rdb_last_bgsave_status:ok

This indicates that the last background RDB save completed successfully.

### AOF

Observed:

    appendonly
    no

Therefore:

    AOF = Disabled

The persistence information also showed:

    aof_enabled:0

Therefore AOF persistence was not active in this environment.

---

# 13. Actual Redis Data Directory

Redis data directory was checked using the Redis configuration.

Observed:

    /var/lib/redis

Therefore the Redis runtime data directory was:

    /var/lib/redis

---

# 14. Actual Redis Database Configuration

The Redis database count was checked.

Observed:

    databases
    16

Therefore the Redis server was configured with:

    16 logical databases

---

# 15. Actual Redis Memory Status

Redis memory usage was inspected.

Observed:

    used_memory_human:1.10M
    maxmemory_human:0B

Interpretation:

    Redis currently uses approximately 1.10 MB memory.

    maxmemory_human:0B

means no explicit Redis maxmemory limit was configured.

---

# 16. Actual Redis Client Status

Client information was inspected.

Observed:

    connected_clients:1
    blocked_clients:0

Interpretation:

- 1 Redis client was connected during the check
- 0 clients were blocked on blocking operations

---

# 17. Actual Redis Statistics

Redis operational statistics were checked.

Observed:

    total_connections_received:39
    total_commands_processed:44
    instantaneous_ops_per_sec:0

These values represent Redis activity observed during the practical session.

---

# 18. Actual Redis Service Logs

Redis logs were inspected using systemd journal logs.

The logs showed successful service startup events:

    Starting redis-server.service
    Started redis-server.service

The Redis service successfully started after multiple system boots during the recorded environment history.

No Redis startup failure was observed in the final verification.

---

# 19. Actual Redis Troubleshooting Verification

The troubleshooting workflow checked:

### Health

    PONG

### Version

    8.0.5

### Process

    2083

### Port

    6379

### Connected Clients

    1

### Blocked Clients

    0

### Memory

    1.10M

### Service

    active

### Boot Enablement

    enabled

### Logs

    Redis service startup successfully recorded

Therefore the Redis environment was operational at the end of troubleshooting.

---

# 20. Actual Redis Final Verification

The final verification confirmed:

    Redis Service
    active

    Redis Service Boot Enablement
    enabled

    Redis Health
    PONG

    Redis Version
    8.0.5

    Redis Port
    6379

    Node.js Redis Dependency
    redis@6.2.1

    Node.js Redis Connection
    Successful

    Redis Cache
    Successful

    Cache TTL
    60 seconds

    MongoDB Container
    mongodb / mongo:8

This confirmed that the main database and caching components were operational together.

---

# 21. Redis and MongoDB Project Architecture

The practical project environment now contains two different data services.

    MERN Application
           |
           +--------------------+
           |                    |
           ↓                    ↓
       MongoDB                Redis
       Docker                Native Service
       mongo:8                 8.0.5
       Port 27017              Port 6379
           |                    |
           ↓                    ↓
    Persistent Data          Cache / Fast Data
    BlogApp Database         Temporary Data

MongoDB remains the primary application database.

Redis was introduced as a supporting fast data store and caching layer.

Redis did not replace MongoDB.

---

# 22. Actual MongoDB Environment Used Alongside Redis

The existing MongoDB container was verified during the Redis final verification.

Observed:

    Container: mongodb
    Image: mongo:8
    Port: 27017

MongoDB was running in Docker while Redis was running as a native Ubuntu service.

Therefore the actual project environment contains:

    MongoDB → Docker
    Redis   → Native systemd service

This distinction is important for future DevOps troubleshooting.

---

# 23. Actual Project Changes

During Redis integration, the backend project was modified.

The final Git status showed:

    M server/package.json
    M server/yarn.lock
    ?? server/.env

The Redis Node.js dependency was added to:

    server/package.json

The Redis package was installed as:

    redis@6.2.1

The `.env` file was present as an untracked file.

The `.env` file contains environment-specific configuration and should not be committed when it contains secrets or machine-specific values.

---

# 24. Actual Final Redis Verification Result

The final Redis cache verification produced:

    OK
    "BlogApp-Cache"
    (integer) 60
    (integer) 1

Interpretation:

    OK
    → Cache value was stored successfully

    "BlogApp-Cache"
    → Cache value was retrieved successfully

    60
    → Cache TTL was configured for 60 seconds

    1
    → Cache key was successfully deleted

Therefore the complete cache lifecycle was verified successfully.

---

# 25. Actual Step 19 Lab Summary

The Step 19 Redis lab followed this workflow:

    Redis Installation
          ↓
    Service Verification
          ↓
    Port Verification
          ↓
    Redis Connectivity
          ↓
    Redis Basic Operations
          ↓
    Redis Data Types
          ↓
    Node.js Redis Client Installation
          ↓
    Node.js Redis Connection
          ↓
    Redis Cache Testing
          ↓
    TTL Verification
          ↓
    Persistence Inspection
          ↓
    Memory Monitoring
          ↓
    Client Monitoring
          ↓
    Statistics Inspection
          ↓
    Log Inspection
          ↓
    Troubleshooting
          ↓
    Final Project Verification

---

# 26. Actual Step 19 Environment Summary

| Component | Actual Value |
|---|---|
| Operating System | Ubuntu Linux |
| Redis Runtime | Native systemd service |
| Redis Service | `redis-server.service` |
| Redis Version | `8.0.5` |
| Redis CLI | `8.0.5` |
| Redis Port | `6379` |
| Redis Bind Address | `127.0.0.1` / `::1` |
| Redis Data Directory | `/var/lib/redis` |
| Redis Databases | `16` |
| RDB | Enabled/configured |
| Last RDB Background Save | `ok` |
| AOF | Disabled |
| Redis Memory Used | `1.10M` |
| Redis Maxmemory | `0B` |
| Connected Clients | `1` |
| Blocked Clients | `0` |
| Node.js Redis Package | `redis@6.2.1` |
| Main Database | MongoDB |
| MongoDB Runtime | Docker |
| MongoDB Image | `mongo:8` |
| MongoDB Container | `mongodb` |
| MongoDB Port | `27017` |

---

# 27. Key Takeaways

### Redis

Redis is being used as a fast supporting data store.

### MongoDB

MongoDB remains the primary persistent application database.

### Node.js

The backend can communicate with Redis using the `redis` Node.js package.

### Caching

Redis can reduce repeated database access by serving frequently requested data from memory.

### TTL

Cache entries can automatically expire after a defined period.

### Persistence

The current Redis environment has RDB snapshot configuration, while AOF is disabled.

### Operations

Redis can be monitored through:

    redis-cli
    systemctl
    ss
    journalctl
    INFO
    CONFIG

### DevOps Perspective

The practical work demonstrated how an application can use multiple infrastructure services with different runtime models:

    MongoDB → Docker
    Redis → systemd

---

# 28. Step 19 Completion

Step 19 — Redis was completed successfully.

The final environment verified:

    ✓ Redis installed
    ✓ Redis service running
    ✓ Redis service enabled
    ✓ Redis port verified
    ✓ Redis connectivity verified
    ✓ Redis String tested
    ✓ Redis List tested
    ✓ Redis Set tested
    ✓ Redis Hash tested
    ✓ Redis CRUD operations tested
    ✓ Redis cache MISS tested
    ✓ Redis cache HIT tested
    ✓ Redis TTL tested
    ✓ Node.js Redis client installed
    ✓ Node.js → Redis connection verified
    ✓ RDB configuration inspected
    ✓ AOF status verified
    ✓ Redis persistence status inspected
    ✓ Redis memory inspected
    ✓ Redis clients inspected
    ✓ Redis statistics inspected
    ✓ Redis logs inspected
    ✓ Troubleshooting workflow completed
    ✓ Final project verification completed

---

# 29. Step 19 Final Status

    STATUS: COMPLETED

Redis is now available in the project environment as a supporting caching and fast-data service alongside the existing MongoDB application database.

The practical implementation, verification, troubleshooting and documentation were completed using the actual project environment.

