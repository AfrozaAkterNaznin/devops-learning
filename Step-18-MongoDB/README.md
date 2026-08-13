# Step 18 — MongoDB

# Part 1 — Concepts, Architecture & Project Structure

## 1. Objective

The objective of Step 18 was to understand and practically implement MongoDB within the existing Blog-App-using-MERN-stack project.

The step covered:

- MongoDB environment verification
- Docker-based MongoDB deployment
- MongoDB database, collection and document structure
- MongoDB Shell operations
- CRUD operations
- Mongoose integration with Node.js
- Project User and Blog models
- Authentication-related database operations
- MongoDB indexes and basic query performance
- Backup and restore
- Persistent Docker storage
- MongoDB configuration and final verification

---

## 2. Learning Outcomes

After completing Step 18, the following practical skills were gained:

- Verify MongoDB running inside a Docker container
- Access MongoDB using `mongosh`
- Identify databases and collections
- Create and manage MongoDB documents
- Perform CRUD operations
- Create and inspect a practice database
- Understand Mongoose as the MongoDB ODM used by the backend
- Understand the relationship between User and Blog documents
- Verify application signup and login database operations
- Understand MongoDB indexes
- Verify index usage through query execution plans
- Create MongoDB backups
- Restore a backup into a temporary database
- Use Docker volumes for persistent MongoDB storage
- Perform final MongoDB project verification

---

## 3. Workflow

The actual Step 18 workflow was:

    Existing Blog-App-using-MERN-stack
                    |
                    v
    MongoDB Environment Verification
                    |
                    v
    Docker MongoDB Container
                    |
                    v
    Database / Collection / Document
                    |
                    v
    MongoDB Shell + CRUD
                    |
                    v
    Practice Database
    devops_practice
                    |
                    v
    Project MongoDB + Mongoose
                    |
                    v
    User / Blog Models
                    |
                    v
    Signup / Login Verification
                    |
                    v
    Indexes + Query Performance
                    |
                    v
    Backup + Restore
                    |
                    v
    MongoDB Configuration
                    |
                    v
    Final Project Verification

---

## 4. Theory

### 4.1 MongoDB

MongoDB is a NoSQL document-oriented database.

Instead of storing data in relational tables and rows, MongoDB stores data as documents inside collections.

Basic structure:

    MongoDB
        |
        +-- Database
                |
                +-- Collection
                        |
                        +-- Document

---

### 4.2 Database

A MongoDB database contains related collections.

The Blog-App application uses:

    BlogApp

A separate practice database was also created:

    devops_practice

---

### 4.3 Collection

A collection is a group of MongoDB documents.

The Blog-App database contains:

    BlogApp
    ├── users
    └── blogs

---

### 4.4 Document

A document stores an individual record in MongoDB.

Example:

    {
      "name": "Afroza",
      "role": "DevOps Learner",
      "stack": "MERN"
    }

MongoDB automatically provides an `_id` field for documents.

---

### 4.5 Mongoose

Mongoose is the ODM used by the Node.js backend to communicate with MongoDB.

The project connection flow is:

    Node.js / Express
           |
           v
        Mongoose
           |
           v
        MongoDB

The project database configuration uses the `MONGO_URI` environment variable.

---

## 5. Architecture Overview

The MongoDB portion of the project uses the following architecture:

    React Client
         |
         v
    Express / Node.js Backend
         |
         v
       Mongoose
         |
         v
       MongoDB
         |
         v
    Docker Container
         |
         v
    Persistent Docker Volume
    mongodb_mongodb_data

MongoDB is not running as a native Ubuntu service in this project.

It runs through Docker using the MongoDB image:

    mongo:8

The MongoDB container is named:

    mongodb

MongoDB is exposed through port:

    27017

---

## 6. Important Concepts

| Concept | Step 18 Implementation |
|---|---|
| MongoDB | Main NoSQL database |
| Docker | MongoDB runtime |
| MongoDB Image | `mongo:8` |
| Container | `mongodb` |
| MongoDB Port | `27017` |
| Database | `BlogApp` |
| Practice Database | `devops_practice` |
| Collections | `users`, `blogs` |
| Shell | `mongosh` |
| ODM | Mongoose |
| Identifier | MongoDB `ObjectId` |
| User Model | `User` |
| Blog Model | `Blog` |
| Index | Unique `email` index |
| Query Analysis | `explain()` |
| Backup | `mongodump` |
| Restore | `mongorestore` |
| Persistent Storage | `mongodb_mongodb_data` |

---

## 7. Important Files

The MongoDB implementation uses these project files:

    Blog-App-using-MERN-stack/
    │
    ├── docker-compose.yaml
    │
    └── server/
        │
        ├── .env
        │
        ├── config/
        │   └── db.js
        │
        ├── model/
        │   ├── User.js
        │   └── Blog.js
        │
        ├── controller/
        │   ├── user-contoller.js
        │   └── blog-controller.js
        │
        └── routes/
            ├── user-routes.js
            └── blog-routes.js

### `server/config/db.js`

Responsible for establishing the Mongoose connection to MongoDB.

### `server/.env`

Contains the project MongoDB connection variable:

    MONGO_URI=mongodb://127.0.0.1:27017/BlogApp

### `server/model/User.js`

Defines the MongoDB User schema.

Important fields include:

    name
    email
    password
    blogs

### `server/model/Blog.js`

Defines the MongoDB Blog schema.

Important fields include:

    title
    desc
    img
    user
    date

---

## 8. Important Directories

| Directory | Purpose |
|---|---|
| `server/` | Node.js backend |
| `server/config/` | Backend configuration |
| `server/model/` | Mongoose models |
| `server/controller/` | Application logic |
| `server/routes/` | API routes |

MongoDB itself runs outside the project source directory as a Docker container.

Its persistent data is stored through the Docker volume:

    mongodb_mongodb_data

---

## 9. Difference Tables

### MongoDB vs Relational Database

| MongoDB | Relational Database |
|---|---|
| Database | Database |
| Collection | Table |
| Document | Row |
| Field | Column |
| ObjectId | Primary-key-style identifier |
| NoSQL | SQL |
| Document-oriented | Table-oriented |

### Native MongoDB vs Project MongoDB

| Native MongoDB Service | Project Implementation |
|---|---|
| Installed directly on OS | Running through Docker |
| `mongod` service | `mongodb` container |
| `systemctl` service management | Docker container management |
| Host installation required | `mongo:8` Docker image |
| Direct `mongosh` access | `docker exec ... mongosh` |

During the lab, `mongod` and `mongosh` were not available as native Ubuntu commands. This confirmed that the project's MongoDB environment was provided through Docker rather than a native MongoDB installation.

---

## 10. Summary Table

| Area | Actual Step 18 Result |
|---|---|
| MongoDB Runtime | Docker |
| Docker Image | `mongo:8` |
| Container | `mongodb` |
| MongoDB Version | `8.2.12` |
| Mongosh Version | `2.9.2` |
| Port | `27017` |
| Application Database | `BlogApp` |
| Collections | `users`, `blogs` |
| Mongoose | Used by backend |
| User Index | Unique `email` index |
| Query Verification | `email_1` index used |
| Backup | Successfully created |
| Restore | Successfully verified |
| Persistent Volume | `mongodb_mongodb_data` |
| Final Verification | Passed |

---

## Part 1 Summary

Step 18 integrated MongoDB into the existing MERN backend through a Docker-based MongoDB environment.

The application uses Mongoose to communicate with MongoDB, with `BlogApp` as the application database and `users` and `blogs` as its main collections.

The practical work also covered CRUD operations, authentication-related database operations, indexing, query performance, backup, restore, persistent storage and final operational verification.

# Step 18 — MongoDB

# Part 2 — Commands, Operations, Troubleshooting & Real-World Usage

## 1. Commands Used

### MongoDB Container Verification

```bash
docker ps --filter "name=mongodb"
docker exec mongodb mongosh --version
docker exec mongodb mongosh --quiet --eval 'db.version()'
docker exec mongodb mongosh --quiet --eval 'db.runCommand({ ping: 1 })'
```

| Command | Purpose |
|---|---|
| `docker ps --filter "name=mongodb"` | Verify MongoDB container status |
| `docker exec mongodb mongosh --version` | Verify MongoDB Shell version |
| `docker exec mongodb mongosh --quiet --eval 'db.version()'` | Verify MongoDB server version |
| `docker exec mongodb mongosh --quiet --eval 'db.runCommand({ ping: 1 })'` | Verify MongoDB responsiveness |

### MongoDB Shell

MongoDB was accessed through the Docker container:

```bash
docker exec -it mongodb mongosh
```

Common shell commands used:

```javascript
show dbs
use BlogApp
db
show collections
db.stats()
```

| Command | Purpose |
|---|---|
| `show dbs` | List available databases |
| `use BlogApp` | Select the application database |
| `db` | Display the current database |
| `show collections` | Display collections |
| `db.stats()` | Display database statistics |

---

## 2. MongoDB Structure

The main project uses the following structure:

```text
MongoDB
└── BlogApp
    ├── users
    │   └── Documents
    └── blogs
        └── Documents
```

Application database:

```text
BlogApp
```

Collections:

```text
users
blogs
```

---

## 3. MongoDB CRUD Operations

A separate database named `devops_practice` was created for practical MongoDB CRUD learning.

### Create Database and Collection

```javascript
use devops_practice
db.createCollection("users")
show collections
```

### Insert One Document

```javascript
db.users.insertOne({
  name: "Afroza",
  role: "DevOps Learner",
  stack: "MERN"
})
```

### Insert Multiple Documents

```javascript
db.users.insertMany([
  {
    name: "Rahim",
    role: "Backend Developer",
    stack: "Node.js"
  },
  {
    name: "Karim",
    role: "DevOps Engineer",
    stack: "Docker"
  }
])
```

### Read Documents

```javascript
db.users.find()
```

Filtered query:

```javascript
db.users.find({
  role: "DevOps Engineer"
})
```

### Update Document

```javascript
db.users.updateOne(
  { name: "Karim" },
  { $set: { role: "Senior DevOps Engineer" } }
)
```

### Delete Document

```javascript
db.users.deleteOne({
  name: "Karim"
})
```

CRUD workflow:

```text
Create
  ↓
Read
  ↓
Update
  ↓
Delete
```

---

## 4. Project MongoDB Configuration

The Blog-App-using-MERN-stack project connects to MongoDB using an environment variable.

```text
MONGO_URI=mongodb://127.0.0.1:27017/BlogApp
```

The MongoDB connection is configured through Mongoose.

Observed connection logic:

```javascript
const mongoose = require("mongoose");
require("dotenv").config();

mongoose.set("strictQuery", false);

mongoose.connect(
  process.env.MONGO_URI || "mongodb://mongo:27017/Blog"
)
.then(() => {
  console.log("connected!");
})
.catch((err) => {
  console.log(err);
});
```

Important:

```text
process.env.MONGO_URI
```

is JavaScript/Node.js syntax.

It must not be executed directly as a Bash command.

---

## 5. Mongoose Models

The project contains two MongoDB models.

### User Model

File:

```text
server/model/User.js
```

User schema fields:

| Field | Type | Configuration |
|---|---|---|
| `name` | String | Required |
| `email` | String | Required, Unique |
| `password` | String | Required, Minimum 6 characters |
| `blogs` | ObjectId Array | References `Blog` |

### Blog Model

File:

```text
server/model/Blog.js
```

Blog schema fields:

| Field | Type | Configuration |
|---|---|---|
| `title` | String | Required |
| `desc` | String | Required |
| `img` | String | Required |
| `user` | ObjectId | References `User` |
| `date` | Date | Defaults to current date |

Relationship:

```text
User
 │
 └── blogs[]
       │
       └── Blog

Blog
 │
 └── user
       │
       └── User
```

---

## 6. Authentication Integration

The project contains user authentication routes.

File:

```text
server/routes/user-routes.js
```

Routes verified during the lab:

| Method | Route | Purpose |
|---|---|---|
| GET | `/api/users/` | Get users |
| POST | `/api/users/signup` | Register a user |
| POST | `/api/users/login` | Authenticate a user |

### Signup Test

```bash
curl -X POST http://localhost:5001/api/users/signup \
-H "Content-Type: application/json" \
-d '{"name":"DevOps User","email":"devops@example.com","password":"password123"}'
```

Observed result:

| Result | Value |
|---|---|
| Status | `201` |
| Message | `User registered successfully` |
| Success | `true` |

The signup request successfully created a user in `BlogApp.users`.

### Login Test

```bash
curl -X POST http://localhost:5001/api/users/login \
-H "Content-Type: application/json" \
-d '{"email":"devops@example.com","password":"password123"}'
```

Observed result:

| Result | Value |
|---|---|
| Status | `200` |
| Message | `Login successful` |
| Success | `true` |

This verified that the Node.js application successfully communicates with MongoDB through Mongoose during authentication.

---

## 7. Blog Database Operations

File:

```text
server/controller/blog-controller.js
```

Important Mongoose operations observed in the project:

```javascript
Blog.find()
```

Retrieves blogs.

```javascript
Blog.findById(id)
```

Retrieves a blog by ID.

```javascript
Blog.findByIdAndUpdate(
  blogId,
  { title, desc },
  { new: true }
)
```

Updates a blog.

```javascript
Blog.findByIdAndDelete(id)
```

Deletes a blog.

The project also uses population for User and Blog relationships:

```javascript
.populate("blogs")
```

and:

```javascript
.populate("user")
```

---

## 8. Indexes

Indexes were verified for both application collections.

### Users Collection

```javascript
db.users.getIndexes()
```

Observed indexes:

```text
_id
email_1
```

The `email_1` index has:

```text
unique: true
```

Therefore duplicate email values are prevented.

### Blogs Collection

```javascript
db.blogs.getIndexes()
```

Observed index:

```text
_id
```

Index summary:

| Collection | Index | Purpose |
|---|---|---|
| `users` | `_id` | Default document identifier |
| `users` | `email_1` | Email lookup and uniqueness |
| `blogs` | `_id` | Default document identifier |

---

## 9. Query Performance

An email query was tested using MongoDB's query planner.

Observed result:

| Metric | Result |
|---|---:|
| Winning Stage | `EXPRESS_IXSCAN` |
| Index Used | `email_1` |
| Keys Examined | `1` |
| Documents Examined | `1` |
| Documents Returned | `1` |

The important observation was that MongoDB used the `email_1` index instead of scanning the collection.

Workflow:

```text
Query
  ↓
MongoDB Query Planner
  ↓
email_1 Index
  ↓
Matching Document
```

---

## 10. Database Statistics

The application database was verified using:

```javascript
db.stats()
```

Observed final state:

| Item | Value |
|---|---:|
| Database | `BlogApp` |
| Collections | `2` |
| Users | `1` |
| Blogs | `0` |

Collections:

```text
users
blogs
```

The registered user was created during the authentication test.

---

## 11. MongoDB Backup

A MongoDB backup was created using `mongodump`.

The backup included:

```text
BlogApp.users
BlogApp.blogs
```

Observed backup file:

```text
BlogApp-20260812-211421.archive.gz
```

At backup time:

| Collection | Documents |
|---|---:|
| `users` | `1` |
| `blogs` | `0` |

The backup completed successfully.

---

## 12. MongoDB Restore

The backup was restored into a temporary database:

```text
BlogApp_restore_test
```

Restore result:

```text
1 document restored
0 documents failed
```

Restored collections:

```text
blogs
users
```

Verification:

| Collection | Documents |
|---|---:|
| `users` | `1` |
| `blogs` | `0` |

The successful restore verified that the backup could be used to recover the application database.

---

## 13. Restore Cleanup

After verification, the temporary restore database was removed.

The temporary local backup directory was also removed.

This prevented temporary restore data and backup files from remaining in the project directory.

---

## 14. Docker MongoDB Configuration

The MongoDB container configuration was inspected during the lab.

Relevant configuration:

| Configuration | Observed Value |
|---|---|
| Container | `mongodb` |
| Image | `mongo:8` |
| Restart Policy | `unless-stopped` |
| Network | `mongodb_default` |
| Port | `27017` |
| MongoDB Data Path | `/data/db` |

Port mapping:

```text
27017/tcp -> 0.0.0.0:27017
27017/tcp -> [::]:27017
```

---

## 15. Persistent MongoDB Storage

MongoDB uses the Docker volume:

```text
mongodb_mongodb_data
```

The volume is mounted to:

```text
/data/db
```

Storage flow:

```text
Docker Volume
      ↓
mongodb_mongodb_data
      ↓
/data/db
      ↓
MongoDB Database Files
```

This provides persistent storage for MongoDB data.

---

## 16. Common Troubleshooting

### MongoDB Command Not Found

The Ubuntu host did not have a native `mongod` or `mongosh` command available.

The project MongoDB instance runs inside Docker.

Correct access method:

```bash
docker exec -it mongodb mongosh
```

### MongoDB systemd Service Not Found

The following service was not available:

```text
mongod.service
```

This is expected because MongoDB is running inside Docker rather than as a native Ubuntu systemd service.

Architecture:

```text
Ubuntu
  ↓
Docker
  ↓
mongodb Container
  ↓
MongoDB Server
```

### Exiting MongoDB Shell

Correct methods:

```text
exit
```

or:

```text
Ctrl+D
```

`q` is not the normal MongoDB Shell exit command.

`Ctrl+Z` suspends the process and should not be used as the normal MongoDB Shell exit method.

### Docker Compose Warning

The project Docker Compose configuration produced a warning that the `version` attribute is obsolete.

The warning did not prevent the MongoDB container from running.

---

## 17. Real-World Uses

MongoDB can be used for:

| Use Case | Example |
|---|---|
| Web applications | MERN applications |
| REST APIs | Node.js backend |
| User data | Profiles and accounts |
| Content systems | Blogs and articles |
| Product data | Product catalogs |
| Event data | Application events |
| Microservices | Service-specific databases |
| Flexible data | Frequently changing document structures |

In this project MongoDB stores:

```text
Users
Blogs
User ↔ Blog relationships
```

---

## 18. Interview Questions

### What is MongoDB?

MongoDB is a NoSQL document-oriented database that stores data as documents inside collections.

### What is a collection?

A collection is a group of MongoDB documents, conceptually similar to a table in a relational database.

### What is a document?

A document is an individual MongoDB record stored in BSON format.

### Why was MongoDB used through Docker?

Docker provides an isolated and reproducible MongoDB environment without requiring a native MongoDB installation on Ubuntu.

### What is Mongoose?

Mongoose is a Node.js ODM that provides schemas, models and database operations for MongoDB.

### Why is the email field unique?

The unique email index prevents multiple users from registering with the same email address.

### What does IXSCAN mean?

`IXSCAN` indicates that MongoDB is using an index to locate matching documents.

### Why are Docker volumes important for MongoDB?

Docker volumes provide persistent storage so database data is not dependent only on the lifetime of a container.

### What is mongodump?

`mongodump` is a MongoDB backup utility used to export database data.

### What is mongorestore?

`mongorestore` restores MongoDB data from a backup.

---

## 19. Part 2 Summary

Step 18 covered the practical MongoDB workflow used in the Blog-App-using-MERN-stack project.

```text
MongoDB Container
       ↓
MongoDB Shell
       ↓
Database / Collections
       ↓
CRUD Operations
       ↓
Mongoose
       ↓
Project Models
       ↓
Authentication
       ↓
Indexes
       ↓
Query Performance
       ↓
Backup
       ↓
Restore
       ↓
Persistent Storage
       ↓
Final Verification
```

Step 18 practical work verified:

| Area | Status |
|---|---|
| MongoDB Docker environment | Verified |
| MongoDB Shell | Practiced |
| Database and collections | Verified |
| CRUD operations | Practiced |
| Mongoose integration | Verified |
| User authentication | Tested |
| Blog/User models | Inspected |
| Indexes | Verified |
| Query performance | Tested |
| Backup | Completed |
| Restore | Completed |
| Persistent volume | Verified |
| Final project database | Verified |



# Step 18 — MongoDB

# Part 3 — Real Lab Summary, Machine Information & Final Review

## 1. Skills Gained

After completing Step 18, the following practical skills were gained:

| Skill | Practical Result |
|---|---|
| MongoDB with Docker | MongoDB container managed through Docker |
| MongoDB Shell | Accessed MongoDB using `mongosh` |
| Database Management | Worked with `BlogApp` and `devops_practice` |
| Collections | Worked with `users` and `blogs` |
| CRUD | Performed insert, read, update and delete operations |
| Mongoose | Inspected project database connection and models |
| Authentication | Tested signup and login APIs |
| Indexing | Inspected MongoDB indexes |
| Query Performance | Verified index usage with query planner |
| Backup | Created MongoDB backup |
| Restore | Restored backup into a temporary database |
| Docker Volumes | Verified persistent MongoDB storage |
| Troubleshooting | Diagnosed Docker/native MongoDB differences |
| Database Verification | Performed final database and container checks |

---

## 2. Real Lab Summary

Step 18 was performed on the main project:

```text
Blog-App-using-MERN-stack
```

The project uses MongoDB as its application database.

Practical workflow:

```text
Docker MongoDB
      ↓
MongoDB Shell
      ↓
BlogApp Database
      ↓
users + blogs Collections
      ↓
Mongoose
      ↓
Node.js Backend
      ↓
Signup / Login / Blog APIs
      ↓
Indexes
      ↓
Backup + Restore
      ↓
Final Verification
```

---

## 3. Real Machine Information

The following information was directly observed during the Step 18 lab.

| Item | Observed Value |
|---|---|
| Host | `afroza@afroza-VirtualBox` |
| Project | `Blog-App-using-MERN-stack` |
| MongoDB Container | `mongodb` |
| MongoDB Image | `mongo:8` |
| MongoDB Server Version | `8.2.12` |
| MongoDB Shell Version | `2.9.2` |
| MongoDB Port | `27017` |
| Main Database | `BlogApp` |
| Collections | `users`, `blogs` |
| Docker Network | `mongodb_default` |
| MongoDB Data Path | `/data/db` |
| Persistent Volume | `mongodb_mongodb_data` |

---

## 4. Files Used

The following project files were inspected during Step 18:

```text
Blog-App-using-MERN-stack/
│
├── server/
│   ├── config/
│   │   └── db.js
│   │
│   ├── model/
│   │   ├── User.js
│   │   └── Blog.js
│   │
│   ├── controller/
│   │   ├── user-contoller.js
│   │   └── blog-controller.js
│   │
│   ├── routes/
│   │   ├── user-routes.js
│   │   └── blog-routes.js
│   │
│   └── .env
│
└── docker-compose.yaml
```

Note:

The actual project file is named:

```text
user-contoller.js
```

and the route imports that existing filename.

---

## 5. Services Used

| Service / Tool | Role |
|---|---|
| Docker | MongoDB runtime |
| MongoDB | Application database |
| Mongosh | MongoDB command-line shell |
| Node.js | Backend runtime |
| Mongoose | MongoDB ODM |
| Express.js | API server |
| MongoDB Docker Volume | Persistent database storage |

---

## 6. Variables Used

The main MongoDB connection variable observed in the project:

```text
MONGO_URI=mongodb://127.0.0.1:27017/BlogApp
```

The application uses:

```javascript
process.env.MONGO_URI
```

for the MongoDB connection.

Fallback connection observed in the code:

```text
mongodb://mongo:27017/Blog
```

The environment variable is preferred when available.

---

## 7. Important Commands

### Docker

```bash
docker ps --filter "name=mongodb"
docker exec -it mongodb mongosh
docker inspect mongodb
docker volume ls
```

### MongoDB Verification

```bash
docker exec mongodb mongosh --version
docker exec mongodb mongosh --quiet --eval 'db.version()'
docker exec mongodb mongosh --quiet --eval 'db.runCommand({ ping: 1 })'
```

### MongoDB Database Operations

```javascript
show dbs
use BlogApp
show collections
db.stats()
```

### CRUD

```javascript
db.users.insertOne(...)
db.users.insertMany(...)
db.users.find()
db.users.updateOne(...)
db.users.deleteOne(...)
```

### Indexes

```javascript
db.users.getIndexes()
db.blogs.getIndexes()
```

### Backup

```bash
mongodump
```

### Restore

```bash
mongorestore
```

---

## 8. Key Observations

### MongoDB Runtime

```text
Container: mongodb
Image: mongo:8
Status: Up
Port: 27017
```

### Application Database

```text
Database: BlogApp
Collections:
  users
  blogs
```

### Final Documents

```text
Users: 1
Blogs: 0
```

The user document was created through the signup API during the authentication test.

---

## 9. Index Observation

Final user indexes:

```text
_id
email_1
```

The `email_1` index was configured as unique.

Final blog indexes:

```text
_id
```

The email query successfully used:

```text
email_1
```

with:

```text
Keys Examined: 1
Documents Examined: 1
Documents Returned: 1
```

This verified that the indexed email lookup was working correctly.

---

## 10. Backup and Restore Result

Backup:

```text
BlogApp
├── users → 1 document
└── blogs → 0 documents
```

Restore:

```text
BlogApp_restore_test
├── users → 1 document
└── blogs → 0 documents
```

Restore result:

```text
1 document restored
0 documents failed
```

After verification:

```text
BlogApp_restore_test
```

was removed.

Temporary backup files were also removed from the project directory.

---

## 11. Final Project Verification

Final verification confirmed:

| Verification | Result |
|---|---|
| MongoDB container | Running |
| MongoDB ping | Successful |
| MongoDB server | `8.2.12` |
| Database | `BlogApp` |
| Collections | `blogs`, `users` |
| Users | `1` |
| Blogs | `0` |
| User indexes | `_id`, `email_1` |
| Blog indexes | `_id` |
| MongoDB volume | `mongodb_mongodb_data` |
| Application connection | Successful |
| Signup API | Successful |
| Login API | Successful |
| Backup | Successful |
| Restore | Successful |
| Temporary restore cleanup | Successful |

---

## 12. Folder Structure

The relevant project structure after Step 18:

```text
Blog-App-using-MERN-stack/
│
├── client/
│
├── server/
│   ├── config/
│   │   └── db.js
│   ├── controller/
│   ├── model/
│   │   ├── Blog.js
│   │   └── User.js
│   ├── routes/
│   ├── .env
│   └── server.js
│
├── docker-compose.yaml
│
└── documentation/
```

Step 18 documentation will later be incorporated into the project's final documentation structure after the Phase 1 work is completed.

---

## 13. Key Takeaways

1. MongoDB is running inside Docker rather than as a native Ubuntu systemd service.

2. The main application database is:

```text
BlogApp
```

3. The application currently contains:

```text
users
blogs
```

4. Node.js communicates with MongoDB through Mongoose.

5. MongoDB CRUD operations were practiced independently using:

```text
devops_practice
```

6. Signup and login were verified against the real application database.

7. The `email_1` unique index was verified and used during an email query.

8. MongoDB backup and restore were successfully tested.

9. MongoDB data is persisted through:

```text
mongodb_mongodb_data
```

10. Final container, database, indexes and storage verification were completed.

---

## 14. Step 18 Completion Status

| Area | Status |
|---|---|
| 18.1 MongoDB Environment + Service | Completed |
| 18.2 Database, Collection, Document | Completed |
| 18.3 MongoDB Shell + CRUD | Completed |
| 18.4 MongoDB + Mongoose | Completed |
| 18.5 Authentication & Permissions | Completed |
| 18.6 Indexes + Query Performance | Completed |
| 18.7 Backup, Restore & Troubleshooting | Completed |
| 18.8 Configuration + Production Considerations | Completed |
| 18.9 Final Project Verification | Completed |

---

## 15. Step Summary

```text
Step 18 — MongoDB
        │
        ├── Docker MongoDB
        │
        ├── MongoDB Shell
        │
        ├── Database & Collections
        │
        ├── CRUD
        │
        ├── Mongoose
        │
        ├── Authentication
        │
        ├── Indexes
        │
        ├── Query Performance
        │
        ├── Backup
        │
        ├── Restore
        │
        ├── Persistent Storage
        │
        └── Final Verification
```

Step 18 is complete from both the practical and verification perspective.

