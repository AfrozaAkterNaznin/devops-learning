# Step 17 — PostgreSQL

## Part 1 — PostgreSQL Fundamentals

---

## 1. Objective

The objective of Step 17 is to develop practical PostgreSQL knowledge for backend development, Linux administration, DevOps, database troubleshooting, application configuration, permissions, authentication, backup, and restore.

This step focuses on practical PostgreSQL usage rather than advanced SQL or database administration.

---

## 2. Learning Outcomes

After completing this step, the learner should be able to:

- Understand PostgreSQL's practical architecture.
- Manage PostgreSQL services and clusters.
- Understand servers, databases, schemas, roles, and tables.
- Create and manage PostgreSQL databases.
- Create users and roles.
- Understand PostgreSQL authentication.
- Use the `psql` command-line interface.
- Create relational tables and relationships.
- Work with primary keys and foreign keys.
- Perform basic CRUD operations.
- Modify existing database structures.
- Understand PostgreSQL permissions.
- Configure and inspect PostgreSQL connection settings.
- Read PostgreSQL logs.
- Perform logical database backup and restore.
- Understand application-to-database connection parameters.
- Compare PostgreSQL with MongoDB at a practical level.

---

## 3. Workflow

```text
PostgreSQL Service
        ↓
PostgreSQL Cluster
        ↓
Role / Authentication
        ↓
Database
        ↓
Schema
        ↓
Tables
        ↓
Relationships
        ↓
Data Operations
        ↓
Permissions
        ↓
Configuration
        ↓
Logs
        ↓
Backup
        ↓
Restore
        ↓
Application Connection
```

---

## 4. PostgreSQL Overview

PostgreSQL is an open-source relational database management system.

It stores structured data using:

- Databases
- Schemas
- Tables
- Rows
- Columns
- Constraints
- Relationships
- Roles and permissions

PostgreSQL is commonly used in backend applications where structured relational data and strong consistency are important.

---

## 5. Practical Architecture Overview

```text
PostgreSQL Server
│
├── Cluster
│   │
│   ├── Database
│   │   │
│   │   ├── Schema
│   │   │   │
│   │   │   ├── Table
│   │   │   │   ├── Rows
│   │   │   │   └── Columns
│   │   │   │
│   │   │   └── Constraints
│   │   │
│   │   └── Database Objects
│   │
│   └── System Databases
│
├── Roles / Users
├── Authentication
├── Configuration
└── Logs
```

---

## 6. PostgreSQL Server vs Database vs Schema

| Concept | Purpose |
|---|---|
| PostgreSQL Server | Running PostgreSQL database service |
| Cluster | PostgreSQL-managed collection of databases and system metadata |
| Database | Logical database environment containing schemas and objects |
| Schema | Namespace used to organize database objects |
| Table | Structured storage for rows and columns |
| Row | One record |
| Column | One attribute of a record |

### Relationship

```text
PostgreSQL Server
       ↓
    Cluster
       ↓
   Database
       ↓
    Schema
       ↓
     Table
       ↓
Rows + Columns
```

---

## 7. PostgreSQL Roles and Users

PostgreSQL uses a role-based security model.

A role can:

- Connect to a database
- Own databases or objects
- Create databases
- Create other roles
- Read data
- Insert data
- Update data
- Delete data
- Perform administrative operations

A login-enabled role can be used as a database user by applications and administrators.

---

## 8. Authentication

PostgreSQL authentication determines whether a client is allowed to connect to the server and how the client identity is verified.

Authentication is controlled primarily through `pg_hba.conf`.

Common authentication concepts include:

| Method | Practical Meaning |
|---|---|
| `peer` | Local Unix socket authentication based on operating-system identity |
| `scram-sha-256` | Password-based authentication using SCRAM |
| `trust` | Allows connection without password verification |
| `reject` | Explicitly rejects a connection |

Authentication and authorization are different:

| Authentication | Authorization |
|---|---|
| Who are you? | What are you allowed to do? |
| Verifies identity | Controls privileges |
| Controlled by authentication rules | Controlled by roles and privileges |

---

## 9. psql CLI

`psql` is PostgreSQL's interactive command-line client.

It can be used to:

- Connect to databases
- Execute SQL
- Inspect databases
- Inspect schemas
- Inspect tables
- Inspect roles
- Check connection information
- Perform administrative tasks

Useful `psql` commands:

| Command | Purpose |
|---|---|
| `\l` | List databases |
| `\du` | List roles |
| `\dn` | List schemas |
| `\dt` | List tables |
| `\d table_name` | Describe a table |
| `\conninfo` | Show current connection |
| `\q` | Exit `psql` |

---

## 10. Database Structure

A PostgreSQL database can organize application data using schemas and tables.

Example:

```text
backend_lab
│
└── app schema
    │
    ├── users
    │
    └── posts
```

The schema provides a namespace that separates application objects from other database objects.

---

## 11. Relational Tables

A relational table consists of rows and columns.

Example:

```text
users

id | full_name | email
---+-----------+--------------------
1  | User One  | user1@example.com
2  | User Two  | user2@example.com
```

Each row represents one record.

Each column represents one attribute.

---

## 12. Primary Key

A primary key uniquely identifies a row in a table.

Example:

```text
users
----------------
id       PRIMARY KEY
full_name
email
```

Important properties:

- Uniquely identifies a row.
- Cannot contain `NULL`.
- Provides a stable reference for related records.

---

## 13. Foreign Key

A foreign key creates a relationship between tables.

Example:

```text
users
-----
id
  ↑
  │
  │ referenced by
  │
posts
-----
user_id
```

A foreign key helps maintain referential integrity between related records.

---

## 14. Basic Relationship

A common backend relationship is:

```text
One User
   │
   ├── Post
   ├── Post
   └── Post
```

This represents a one-to-many relationship:

```text
users.id
   ↓
posts.user_id
```

---

## 15. Constraints

Constraints enforce rules on database data.

Common PostgreSQL constraints include:

| Constraint | Purpose |
|---|---|
| `PRIMARY KEY` | Uniquely identifies rows |
| `FOREIGN KEY` | Maintains relationships |
| `NOT NULL` | Requires a value |
| `UNIQUE` | Prevents duplicate values |
| `DEFAULT` | Provides an automatic value |
| `CHECK` | Enforces a condition |

Constraints help protect data integrity at the database level.

---

## 16. CRUD Operations

Backend applications commonly perform four basic data operations:

| Operation | SQL |
|---|---|
| Create | `INSERT` |
| Read | `SELECT` |
| Update | `UPDATE` |
| Delete | `DELETE` |

These operations form the basic data interaction layer between an application and a relational database.

---

## 17. Permissions

PostgreSQL uses privileges to control what roles can do.

Examples:

```text
CONNECT
USAGE
SELECT
INSERT
UPDATE
DELETE
```

A practical security model is to give a role only the permissions it requires.

Example:

```text
Application Reporting Role
        │
        └── SELECT only
```

This is an example of the least-privilege principle.

---

## 18. PostgreSQL Configuration

Important PostgreSQL configuration concepts include:

| Configuration | Purpose |
|---|---|
| `postgresql.conf` | Main PostgreSQL server configuration |
| `pg_hba.conf` | Client authentication rules |
| `port` | TCP listening port |
| `listen_addresses` | Network addresses PostgreSQL listens on |
| `data_directory` | PostgreSQL cluster data location |

Configuration determines important server behavior, networking, and authentication rules.

---

## 19. PostgreSQL Logs

PostgreSQL logs provide operational information about the database server.

Logs can contain:

- Connection events
- Authentication events
- Errors
- Permission failures
- Checkpoints
- Server activity
- Troubleshooting information

Logs are important for diagnosing backend and production database problems.

---

## 20. Backup and Restore

PostgreSQL supports logical backup and restore.

A logical backup represents database objects and data in a format that can be restored into another database environment.

Basic workflow:

```text
Database
   ↓
pg_dump
   ↓
Backup
   ↓
Restore
   ↓
New Database
   ↓
Verification
```

Backup is important for:

- Disaster recovery
- Migration
- Testing
- Development environments
- Database recovery

---

## 21. Application Connection Concept

A backend application generally needs database connection information such as:

| Parameter | Purpose |
|---|---|
| Host | PostgreSQL server address |
| Port | PostgreSQL network port |
| Database | Target database |
| Username | PostgreSQL role |
| Password | Authentication credential |

Conceptually:

```text
Backend Application
        │
        ├── Host
        ├── Port
        ├── Database
        ├── User
        └── Password
                ↓
          PostgreSQL
```

These values are commonly provided to backend applications through environment variables or application configuration.

---

## 22. PostgreSQL vs MongoDB

| Feature | PostgreSQL | MongoDB |
|---|---|---|
| Database model | Relational | Document-oriented |
| Primary structure | Tables | Collections |
| Record | Row | Document |
| Schema | Structured | Flexible |
| Relationships | Native relational relationships | Usually embedded or referenced documents |
| Query language | SQL | MongoDB query/document API |
| Primary key | Primary key | `_id` |
| Strong relational constraints | Yes | More application-oriented |
| Typical strength | Structured relational data | Flexible document data |

### Practical Roadmap Context

```text
Step 17
PostgreSQL
    ↓
Supplementary Database Lab

Step 18
MongoDB
    ↓
Main MERN Project
```

PostgreSQL is practiced separately in Step 17, while MongoDB will be used with the existing MERN project in Step 18.

---

## 23. Summary

Step 17 establishes practical PostgreSQL knowledge required for backend and DevOps work.

The main concepts are:

```text
Service
Cluster
Role
Authentication
Database
Schema
Table
Primary Key
Foreign Key
Constraints
CRUD
Permissions
Configuration
Logs
Backup
Restore
Application Connection
```

The focus is operational PostgreSQL knowledge rather than advanced SQL or database administration.

# Step 17 — PostgreSQL

## Part 2 — Commands, Operations & Practical Administration

---

## 1. Commands Used

### PostgreSQL Version

```bash
psql --version
```

Purpose:

- Verify PostgreSQL client installation.
- Check the installed PostgreSQL version.

---

### PostgreSQL Service

```bash
systemctl status postgresql
systemctl is-active postgresql
systemctl is-enabled postgresql
```

Purpose:

- Check PostgreSQL service status.
- Verify whether the service is active.
- Verify whether the service is enabled at boot.

---

### PostgreSQL Cluster

```bash
pg_lsclusters
```

Purpose:

- List PostgreSQL clusters.
- Check cluster version, name, port, status, owner, data directory and log file.

---

### Cluster Service

```bash
sudo systemctl status postgresql@18-main
```

Purpose:

- Check the actual PostgreSQL 18 `main` cluster service.
- Verify that the database server process is running.

---

### PostgreSQL Processes

```bash
ps aux | grep '[p]ostgres'
```

Purpose:

- View PostgreSQL server and background processes.

---

### Listening Port

```bash
sudo ss -lntp | grep postgres
```

Purpose:

- Check whether PostgreSQL is listening for TCP connections.
- Identify the listening port and process.

---

## 2. PostgreSQL CLI Commands

### List Databases

```bash
sudo -u postgres psql -c '\l'
```

### List Roles

```bash
sudo -u postgres psql -c '\du'
```

### List Schemas

```bash
psql -h 127.0.0.1 -U devops_lab -d backend_lab -c '\dn'
```

### List Tables

```bash
psql -h 127.0.0.1 -U devops_lab -d backend_lab -c '\dt app.*'
```

### Describe a Table

```bash
psql -h 127.0.0.1 -U devops_lab -d backend_lab -c '\d app.users'
```

### Show Connection Information

```bash
psql -h 127.0.0.1 -U devops_lab -d backend_lab -c '\conninfo'
```

### Exit psql

```text
\q
```

---

## 3. Role Management

### Create a Login Role

```sql
CREATE ROLE devops_lab LOGIN PASSWORD 'password';
```

### Create a Read-Only Role

```sql
CREATE ROLE reporting_user LOGIN PASSWORD 'password';
```

### Inspect a Role

```bash
sudo -u postgres psql -c '\du reporting_user'
```

Roles can be used to separate administrative access from application or reporting access.

---

## 4. Database Management

### Create Database

```bash
sudo -u postgres createdb -O devops_lab backend_lab
```

or:

```sql
CREATE DATABASE backend_lab OWNER devops_lab;
```

### Connect to Database

```bash
psql -h 127.0.0.1 -U devops_lab -d backend_lab
```

### Rename Database

```sql
ALTER DATABASE database_name RENAME TO new_database_name;
```

### Drop Database

```bash
sudo -u postgres dropdb database_name
```

A database should only be dropped after confirming that it is no longer required.

---

## 5. Schema Management

### Create Schema

```sql
CREATE SCHEMA app;
```

### List Schemas

```text
\dn
```

### Drop Schema

```sql
DROP SCHEMA schema_name;
```

A schema provides a namespace for organizing database objects.

---

## 6. Table Management

### Create Table

```sql
CREATE TABLE app.users (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

### Create Related Table

```sql
CREATE TABLE app.posts (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    user_id BIGINT NOT NULL,
    title VARCHAR(200) NOT NULL,
    content TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_posts_user
        FOREIGN KEY (user_id)
        REFERENCES app.users(id)
);
```

### Describe Table

```text
\d app.users
\d app.posts
```

---

## 7. ALTER TABLE Operations

### Add Column

```sql
ALTER TABLE app.users
ADD COLUMN bio TEXT;
```

### Rename Column

```sql
ALTER TABLE app.users
RENAME COLUMN name TO full_name;
```

### Add Column with Default

```sql
ALTER TABLE app.users
ADD COLUMN status VARCHAR(20) NOT NULL DEFAULT 'active';
```

### Drop Column

```sql
ALTER TABLE app.users
DROP COLUMN temporary_note;
```

`ALTER TABLE` is used to modify an existing table structure without recreating the entire table.

---

## 8. INSERT

### Insert One Row

```sql
INSERT INTO app.users (full_name, email)
VALUES ('User One', 'user1@example.com');
```

### Insert Multiple Rows

```sql
INSERT INTO app.users (full_name, email)
VALUES
    ('User One', 'user1@example.com'),
    ('User Two', 'user2@example.com');
```

### Insert Related Data

```sql
INSERT INTO app.posts (user_id, title, content)
VALUES (1, 'My First Post', 'Post content');
```

The `user_id` must reference an existing `users.id` because of the foreign key constraint.

---

## 9. SELECT

### Select All Rows

```sql
SELECT * FROM app.users;
```

### Select Specific Columns

```sql
SELECT id, full_name, email
FROM app.users;
```

### Filter Rows

```sql
SELECT *
FROM app.users
WHERE email = 'user1@example.com';
```

### Sort Results

```sql
SELECT *
FROM app.users
ORDER BY id;
```

---

## 10. UPDATE

### Update a Row

```sql
UPDATE app.users
SET full_name = 'Updated Name'
WHERE id = 1;
```

### Verify Update

```sql
SELECT id, full_name, email
FROM app.users
WHERE id = 1;
```

Always use an appropriate `WHERE` condition when updating selected rows.

---

## 11. DELETE

### Delete a Row

```sql
DELETE FROM app.posts
WHERE id = 1;
```

### Verify Delete

```sql
SELECT *
FROM app.posts;
```

A missing or incorrect `WHERE` condition can affect more rows than intended.

---

## 12. Permissions

### Grant Database Connection

```sql
GRANT CONNECT ON DATABASE backend_lab
TO reporting_user;
```

### Grant Schema Usage

```sql
GRANT USAGE ON SCHEMA app
TO reporting_user;
```

### Grant Read Access

```sql
GRANT SELECT
ON ALL TABLES IN SCHEMA app
TO reporting_user;
```

### Revoke Permission

```sql
REVOKE SELECT
ON ALL TABLES IN SCHEMA app
FROM reporting_user;
```

Permissions should follow the least-privilege principle.

---

## 13. Permission Verification

### Database Privileges

```text
\l+ backend_lab
```

### Schema Privileges

```text
\dn+ app
```

### Table Privileges

```text
\dp app.*
```

These commands help verify whether a role has the expected access.

---

## 14. Configuration Inspection

### Show Main Configuration File

```sql
SHOW config_file;
```

### Show Authentication File

```sql
SHOW hba_file;
```

### Show Data Directory

```sql
SHOW data_directory;
```

### Show Port

```sql
SHOW port;
```

### Show Listening Addresses

```sql
SHOW listen_addresses;
```

Administrative configuration values may require elevated PostgreSQL privileges to inspect.

---

## 15. Authentication Configuration

The authentication configuration file can be displayed with:

```bash
sudo cat "$(sudo -u postgres psql --pset=pager=off -Atc 'SHOW hba_file;')"
```

Important rule examples:

```text
local   all   postgres                     peer
local   all   all                          peer
host    all   all   127.0.0.1/32           scram-sha-256
host    all   all   ::1/128                scram-sha-256
```

The first matching `pg_hba.conf` rule determines how a connection is authenticated.

---

## 16. Logs

### PostgreSQL Log Directory

```bash
sudo ls -lh /var/log/postgresql/
```

### View Recent Log Entries

```bash
sudo tail -n 20 /var/log/postgresql/postgresql-18-main.log
```

Logs can reveal:

- Connection attempts
- Authentication failures
- Permission errors
- Server events
- Checkpoints
- Operational problems

---

## 17. Connectivity Troubleshooting

### Check PostgreSQL Readiness

```bash
pg_isready -h 127.0.0.1 -p 5432 -d backend_lab -U devops_lab
```

Expected healthy state:

```text
127.0.0.1:5432 - accepting connections
```

### Check Listening Port

```bash
sudo ss -lntp | grep postgres
```

### Check Cluster

```bash
pg_lsclusters
```

### Check Cluster Service

```bash
systemctl is-active postgresql@18-main
```

These commands help distinguish between:

- Service failure
- Cluster failure
- Port/listening problems
- Authentication problems
- Database access problems

---

## 18. Backup

### Create Backup Directory

```bash
mkdir -p backup
```

### Create Plain SQL Backup

```bash
pg_dump \
  -h 127.0.0.1 \
  -p 5432 \
  -U devops_lab \
  -d backend_lab \
  -F p \
  -f backup/backend_lab.sql
```

### Verify Backup

```bash
ls -lh backup/backend_lab.sql
```

### Inspect Backup

```bash
head -n 20 backup/backend_lab.sql
```

`pg_dump` creates a logical backup of the database.

---

## 19. Restore

### Create Restore Database

```bash
sudo -u postgres createdb -O devops_lab backend_lab_restore
```

### Restore SQL Backup

```bash
psql \
  -h 127.0.0.1 \
  -U devops_lab \
  -d backend_lab_restore \
  -f backup/backend_lab.sql
```

### Verify Restored Schema

```text
\dn
```

### Verify Restored Tables

```text
\dt app.*
```

### Verify Restored Data

```sql
SELECT * FROM app.users;
SELECT * FROM app.posts;
```

### Remove Temporary Restore Database

```bash
sudo -u postgres dropdb backend_lab_restore
```

A restore should be verified rather than assuming that a backup file is valid simply because it was created successfully.

---

## 20. Application Connection

A backend application commonly needs:

```text
Host
Port
Database
Username
Password
```

Example environment variables:

```text
DB_HOST=127.0.0.1
DB_PORT=5432
DB_NAME=backend_lab
DB_USER=devops_lab
DB_PASSWORD=<credential>
```

Example connection concept:

```text
Backend Application
        │
        │ Host: 127.0.0.1
        │ Port: 5432
        │ Database: backend_lab
        │ User: devops_lab
        │ Password: credential
        ↓
PostgreSQL
```

Credentials should be managed securely in real applications and should not be hard-coded into source code.

---

## 21. Command Variations and Useful Options

| Command / Option | Purpose |
|---|---|
| `-h` | Specify PostgreSQL host |
| `-p` | Specify PostgreSQL port |
| `-U` | Specify PostgreSQL user |
| `-d` | Specify database |
| `-c` | Execute a command and exit |
| `-f` | Read SQL commands from a file |
| `-F p` | Use plain SQL dump format |
| `--pset=pager=off` | Disable output pager |
| `-A` | Unaligned output |
| `-t` | Tuples-only output |
| `-At` | Useful for extracting a single value |
| `\l` | List databases |
| `\du` | List roles |
| `\dn` | List schemas |
| `\dt` | List tables |
| `\d` | Describe database objects |
| `\dp` | Show table privileges |
| `\conninfo` | Show current connection |

---

## 22. Common Mistakes

### 1. Confusing the Lab Directory with PostgreSQL Data

```text
~/Projects/devops-supplementary-labs/Step-17-PostgreSQL/
```

is the practical lab workspace.

The PostgreSQL data directory is managed separately by PostgreSQL.

Example:

```text
/var/lib/postgresql/18/main
```

Database files should not be manually edited.

---

### 2. Confusing `postgresql.service` with the Actual Cluster

On Debian/Ubuntu systems, the umbrella service can show:

```text
active (exited)
```

while the actual cluster service is:

```text
postgresql@18-main.service
active (running)
```

`pg_lsclusters` and the cluster-specific service are useful for verifying the actual PostgreSQL cluster.

---

### 3. Bash History Expansion with `!`

A password containing `!` can cause Bash history expansion problems when used directly in shell commands.

Example problem:

```text
bash: !': event not found
```

This can happen before PostgreSQL receives the command.

---

### 4. Using a Non-Administrative Role for Server Settings

A normal database role may not be allowed to inspect some server-level settings.

For example:

```text
permission denied to examine "config_file"
```

This does not necessarily mean PostgreSQL is broken.

---

### 5. Forgetting `WHERE`

Dangerous:

```sql
UPDATE app.users
SET status = 'inactive';
```

This updates every row.

Similarly:

```sql
DELETE FROM app.users;
```

deletes all rows from the table.

Always verify the target rows before destructive operations.

---

### 6. Assuming Backup Means Restore Was Tested

Creating:

```text
backend_lab.sql
```

only proves that a backup file was generated.

A real backup workflow should include:

```text
Backup
 ↓
Restore
 ↓
Verify
```

---

### 7. Giving Applications Superuser Access

Application roles should normally not be PostgreSQL superusers.

A restricted role is safer because database permissions limit what the application can do.

---

## 23. Real-World Uses

| PostgreSQL Skill | Real-World Use |
|---|---|
| Service management | Start, stop and troubleshoot PostgreSQL |
| `pg_lsclusters` | Inspect PostgreSQL clusters |
| Roles | Application and administrative access |
| Authentication | Secure database connections |
| Schemas | Organize application objects |
| Tables | Store structured application data |
| Constraints | Protect data integrity |
| CRUD | Backend data operations |
| Foreign keys | Maintain relationships |
| Permissions | Implement least privilege |
| `pg_hba.conf` | Control client authentication |
| `postgresql.conf` | Configure PostgreSQL server |
| Logs | Troubleshoot database problems |
| `pg_isready` | Check database readiness |
| `pg_dump` | Create logical backups |
| Restore | Recover or migrate databases |
| Environment variables | Connect backend applications securely |

---

## 24. Interview Questions

### Fundamentals

1. What is PostgreSQL?
2. What is the difference between PostgreSQL and a PostgreSQL database?
3. What is a PostgreSQL cluster?
4. What is a schema?
5. What is the difference between a database and a schema?
6. What is `psql`?

### Roles and Authentication

7. What is a PostgreSQL role?
8. What is the difference between authentication and authorization?
9. What is `pg_hba.conf`?
10. What is `peer` authentication?
11. What is `scram-sha-256`?
12. Why should an application avoid using a superuser?

### Database and SQL

13. What is a primary key?
14. What is a foreign key?
15. What is a one-to-many relationship?
16. What is the purpose of `NOT NULL`?
17. What is the purpose of `UNIQUE`?
18. What does `DEFAULT` do?
19. What are CRUD operations?
20. What is the difference between `DELETE` and dropping a table?

### Operations and DevOps

21. How do you check whether PostgreSQL is running?
22. How do you check the PostgreSQL cluster status?
23. How do you check which port PostgreSQL is listening on?
24. Where are PostgreSQL logs located?
25. What is `pg_isready` used for?
26. What is `pg_dump`?
27. Why should a backup be restored and tested?
28. What information does a backend application need to connect to PostgreSQL?
29. What is the purpose of `postgresql.conf`?
30. What is the purpose of `pg_hba.conf`?

---

## 25. Configuration Files

| File | Purpose |
|---|---|
| `postgresql.conf` | Main PostgreSQL server configuration |
| `pg_hba.conf` | Client authentication configuration |
| PostgreSQL log file | Operational and error logging |

### Configuration Locations

Typical Debian/Ubuntu PostgreSQL configuration structure:

```text
/etc/postgresql/<version>/<cluster>/
├── postgresql.conf
├── pg_hba.conf
└── pg_ident.conf
```

The exact paths should always be verified on the actual machine rather than assumed.

---

## Part 2 Summary

Part 2 covered the commands and operational techniques used during the PostgreSQL lab.

The main command areas were:

```text
Service Management
Cluster Management
psql
Roles
Databases
Schemas
Tables
CRUD
ALTER TABLE
Permissions
Authentication
Configuration
Logs
Troubleshooting
Backup
Restore
Application Connection
```

The emphasis is on practical PostgreSQL administration and backend/DevOps usage rather than advanced SQL.

---

# Part 3 — Real Lab Implementation

## 1. Skills Gained

The following practical skills were gained during Step 17:

- PostgreSQL installation and environment verification
- PostgreSQL service and cluster verification
- PostgreSQL process inspection
- PostgreSQL port verification
- PostgreSQL role management
- PostgreSQL authentication
- Database creation and connection
- Schema creation
- Relational table creation
- Primary key implementation
- Foreign key implementation
- One-to-many relationship implementation
- CRUD operations
- Table structure modification
- Database and schema lifecycle operations
- Role-based permissions
- Read-only database access
- PostgreSQL configuration inspection
- `pg_hba.conf` inspection
- PostgreSQL log analysis
- Database connectivity troubleshooting
- Logical database backup
- Database restore
- Application-style PostgreSQL connection
- Final database verification and cleanup

---

## 2. Real Lab Summary

| Item | Actual Lab Result |
|---|---|
| PostgreSQL Version | 18.4 |
| PostgreSQL Cluster | `18/main` |
| Cluster Status | `online` |
| PostgreSQL Port | `5432` |
| Listen Address | `localhost` |
| Main Database | `backend_lab` |
| Database Owner | `devops_lab` |
| Application Schema | `app` |
| Main Tables | `users`, `posts` |
| Main Application Role | `devops_lab` |
| Read-Only Role | `reporting_user` |
| Backup Format | Plain SQL |
| Backup File | `backup/backend_lab.sql` |
| Restore Database | `backend_lab_restore` |
| Restore Verification | Successful |
| Final PostgreSQL Service | Active |
| Final Connectivity | Accepting connections |

---

## 3. Real Machine Information

The Step 17 PostgreSQL lab was performed on the Ubuntu VirtualBox environment.

### PostgreSQL Environment

| Item | Actual Value |
|---|---|
| PostgreSQL Version | `18.4` |
| PostgreSQL Cluster | `18/main` |
| PostgreSQL Data Directory | `/var/lib/postgresql/18/main` |
| PostgreSQL Configuration Directory | `/etc/postgresql/18/main` |
| PostgreSQL Main Configuration | `/etc/postgresql/18/main/postgresql.conf` |
| PostgreSQL Authentication Configuration | `/etc/postgresql/18/main/pg_hba.conf` |
| PostgreSQL Log File | `/var/log/postgresql/postgresql-18-main.log` |
| PostgreSQL Port | `5432` |
| Listen Address | `localhost` |

---

## 4. Practice Directory

The Step 17 PostgreSQL practical lab was stored separately from the documentation repository.

### Practice Path

```text
/home/afroza/Projects/devops-supplementary-labs/Step-17-PostgreSQL
```

### Documentation Path

```text
/home/afroza/Projects/devops-learning/Step-17-PostgreSQL
```

The practice directory contains PostgreSQL lab artifacts, while the documentation directory contains the GitHub-ready documentation.

---

## 5. Files Used

### PostgreSQL Configuration Files

```text
/etc/postgresql/18/main/postgresql.conf
/etc/postgresql/18/main/pg_hba.conf
```

### PostgreSQL Log File

```text
/var/log/postgresql/postgresql-18-main.log
```

### PostgreSQL Data Directory

```text
/var/lib/postgresql/18/main
```

### Lab Backup File

```text
~/Projects/devops-supplementary-labs/Step-17-PostgreSQL/backup/backend_lab.sql
```

The backup file was generated using `pg_dump` during the practical lab.

---

## 6. Services Used

### PostgreSQL Main Service

```text
postgresql.service
```

### PostgreSQL Cluster Service

```text
postgresql@18-main.service
```

The actual PostgreSQL 18 cluster was verified as:

```text
18 main 5432 online postgres
```

The cluster service was verified as active and running.

---

## 7. PostgreSQL Database Structure

The main practical database was:

```text
backend_lab
└── app
    ├── users
    └── posts
```

### Users Table

The `users` table contained:

```text
id
full_name
email
bio
status
created_at
```

### Posts Table

The `posts` table contained:

```text
id
user_id
title
content
created_at
```

The `posts.user_id` column referenced:

```text
users.id
```

through the foreign key:

```text
fk_posts_user
```

---

## 8. Roles Used

### Main Lab Role

```text
devops_lab
```

This role was used for normal database operations and application-style connections.

### Read-Only Role

```text
reporting_user
```

This role was created to demonstrate least-privilege access.

The role was granted:

```text
CONNECT
USAGE
SELECT
```

The role successfully read application data but was denied write access.

---

## 9. Variables Used

The application-style connection test used:

```text
PGHOST=127.0.0.1
PGPORT=5432
PGDATABASE=backend_lab
PGUSER=devops_lab
PGPASSWORD=<configured lab credential>
```

These variables represent the connection information normally required by a backend application.

The password value is intentionally not documented here.

---

## 10. Important Commands

### Environment Verification

```bash
psql --version
pg_lsclusters
systemctl status postgresql
systemctl status postgresql@18-main
```

### Process Verification

```bash
ps aux | grep '[p]ostgres'
```

### Port Verification

```bash
sudo ss -lntp | grep postgres
```

### Database and Role Inspection

```text
\l
\du
\dn
\dt
\d
\dp
\conninfo
```

### Connectivity

```bash
pg_isready -h 127.0.0.1 -p 5432 -d backend_lab -U devops_lab
```

### Backup

```bash
pg_dump \
  -h 127.0.0.1 \
  -p 5432 \
  -U devops_lab \
  -d backend_lab \
  -F p \
  -f backup/backend_lab.sql
```

### Restore

```bash
psql \
  -h 127.0.0.1 \
  -U devops_lab \
  -d backend_lab_restore \
  -f backup/backend_lab.sql
```

### Log Inspection

```bash
sudo tail -n 20 /var/log/postgresql/postgresql-18-main.log
```

---

## 11. Important Observations

### PostgreSQL Cluster

The PostgreSQL cluster was verified as:

```text
18 main 5432 online postgres
```

The cluster was running under:

```text
postgresql@18-main.service
```

---

### PostgreSQL Network

The PostgreSQL server was listening on:

```text
127.0.0.1:5432
```

The configured listen address was:

```text
localhost
```

---

### Authentication

The actual `pg_hba.conf` contained local Unix socket authentication using:

```text
peer
```

Local TCP connections used:

```text
scram-sha-256
```

for IPv4 and IPv6 localhost connections.

---

### Permissions

The `reporting_user` role successfully performed:

```text
SELECT
```

but an `INSERT` attempt failed with:

```text
permission denied for table users
```

This demonstrated practical role-based access control and least privilege.

---

### Logs

The PostgreSQL log recorded:

- Permission failures
- Configuration inspection permission errors
- Checkpoint activity
- Connection/authentication information

The logs were used as an operational troubleshooting source.

---

## 12. Backup and Restore Result

A logical backup of `backend_lab` was created:

```text
backup/backend_lab.sql
```

The backup file size observed during the lab was:

```text
4.1K
```

The backup was restored into:

```text
backend_lab_restore
```

The restored database was verified to contain:

```text
app
├── users
└── posts
```

The restored data and relationship structure were successfully verified.

The temporary restore database was then removed.

---

## 13. Application Connection Result

The application-style PostgreSQL connection was successfully established using:

```text
Host       = 127.0.0.1
Port       = 5432
Database   = backend_lab
User       = devops_lab
```

The actual connection verification showed:

```text
Database             = backend_lab
Client User          = devops_lab
Host                 = 127.0.0.1
Server Port          = 5432
Password Used        = true
SSL Connection       = true
SSL Protocol         = TLSv1.3
Superuser            = off
```

The connection identity was verified using:

```text
current_user     = devops_lab
current_database = backend_lab
```

Application data was successfully read from:

```text
app.users
```

---

## 14. Final Folder Structure

The final practical Step 17 directory contained:

```text
Step-17-PostgreSQL/
└── backup/
    └── backend_lab.sql
```

The PostgreSQL database itself is managed by PostgreSQL under its configured data directory and is not stored as normal editable files inside the practice directory.

---

## 15. Step Summary

Step 17 provided a practical PostgreSQL foundation for backend and DevOps work.

The lab covered the complete lifecycle:

```text
Installation
    ↓
Environment Verification
    ↓
Service / Cluster
    ↓
Roles
    ↓
Authentication
    ↓
Database
    ↓
Schema
    ↓
Tables
    ↓
Relationships
    ↓
CRUD
    ↓
Structure Modification
    ↓
Permissions
    ↓
Configuration
    ↓
Logs
    ↓
Connectivity
    ↓
Backup
    ↓
Restore
    ↓
Application Connection
    ↓
Final Verification
```

---

## 16. Key Takeaways

- PostgreSQL is a relational database system suitable for structured backend data.
- A PostgreSQL cluster can contain multiple databases.
- Schemas organize database objects.
- Roles control authentication and authorization.
- `pg_hba.conf` controls client authentication rules.
- `postgresql.conf` contains main server configuration.
- Primary keys identify records.
- Foreign keys establish relationships.
- Constraints protect data integrity.
- `GRANT` and `REVOKE` control database privileges.
- Read-only roles are useful for limiting application access.
- PostgreSQL logs are important for troubleshooting.
- `pg_isready` is useful for checking database readiness.
- `pg_dump` creates logical database backups.
- A backup should be restored and verified rather than simply assumed to be valid.
- Backend applications need database connection parameters such as host, port, database, user, and password.
- PostgreSQL database files should be managed by PostgreSQL rather than manually edited.
- PostgreSQL and MongoDB solve different types of data-storage problems.

