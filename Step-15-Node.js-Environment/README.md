# Step 15 — Node.js Environment

## Part 1 — Foundation, Architecture and Concepts

---

## 1. Objective

The objective of this step is to understand and configure the Node.js development environment required for backend development and to apply that environment to a real MERN application.

The practical work was performed using the `Blog-App-using-MERN-stack` project.

The step focused on:

- Verifying the existing Node.js environment
- Understanding NVM and npm
- Understanding project-level dependencies
- Installing backend and frontend dependencies
- Configuring environment variables
- Understanding the Node.js backend structure
- Running and verifying the backend
- Running and verifying the React frontend
- Understanding the local MERN application workflow

---

## 2. Learning Outcomes

After completing this step, the following concepts and practical skills were verified:

- Understand the role of Node.js as a JavaScript runtime.
- Understand the role of npm as a package manager.
- Understand NVM for Node.js version management.
- Identify Node.js and npm versions.
- Understand `package.json`.
- Understand project-level dependencies.
- Install dependencies using `npm install`.
- Understand the purpose of `node_modules`.
- Understand `.env` and environment variables.
- Run a Node.js backend using an npm script.
- Understand the relationship between Node.js, Express, and Mongoose.
- Verify a backend HTTP endpoint using `curl`.
- Run a React frontend using npm.
- Understand the basic local architecture of a MERN application.

---

## 3. Workflow

The practical workflow followed during this step was:

```text
GitHub Repository
        |
        v
Clone Project
        |
        v
Inspect Project Structure
        |
        v
Verify Node.js + npm
        |
        v
Inspect package.json
        |
        v
Install Dependencies
        |
        v
Configure .env
        |
        v
Start Node.js Backend
        |
        v
Verify MongoDB Connection
        |
        v
Test Backend API
        |
        v
Start React Frontend
        |
        v
Verify Application in Browser

# Step 15 — Node.js Environment

## Part 2 — Commands, Configuration and Practical Operations

---

## 1. Commands Used

| Command              | Purpose                                       |
| -------------------- | --------------------------------------------- |
| `node --version`     | Check installed Node.js version               |
| `npm --version`      | Check npm version                             |
| `which node`         | Show Node.js executable location              |
| `which npm`          | Show npm executable location                  |
| `nvm current`        | Show active Node.js version                   |
| `nvm ls`             | List installed/available NVM-managed versions |
| `git clone`          | Clone the GitHub repository                   |
| `ls -la`             | Inspect project files and directories         |
| `find`               | Inspect project structure                     |
| `cat`                | Display file contents                         |
| `npm install`        | Install project dependencies                  |
| `npm list --depth=0` | Display direct installed dependencies         |
| `npm start`          | Run the project's start script                |
| `cp`                 | Create `.env` from the example file           |
| `curl`               | Test the backend HTTP endpoint                |
| `cd`                 | Navigate between project directories          |

---

## 2. Command Variations

### Node.js Version

```bash
node --version
node -v
```

Both commands display the installed Node.js version.

### npm Version

```bash
npm --version
npm -v
```

Both commands display the installed npm version.

### NVM Version Inspection

```bash
nvm current
nvm ls
```

`nvm current` shows the active version, while `nvm ls` displays the available NVM-managed versions.

### Dependency Installation

```bash
npm install
npm i
```

`npm i` is the short form of `npm install`.

### Dependency Inspection

```bash
npm list --depth=0
```

`--depth=0` limits the output to direct project dependencies instead of displaying the complete dependency tree.

### API Verification

```bash
curl http://localhost:5001/api
```

Headers and status information can be displayed with:

```bash
curl -i http://localhost:5001/api
```

---

## 3. Important Options and Flags

| Option / Flag | Command              | Purpose                                     |
| ------------- | -------------------- | ------------------------------------------- |
| `-v`          | `node -v`            | Short version option                        |
| `-v`          | `npm -v`             | Short version option                        |
| `-la`         | `ls -la`             | Show hidden files with detailed information |
| `--depth=0`   | `npm list --depth=0` | Show only direct dependencies               |
| `-i`          | `curl -i`            | Include HTTP response headers               |

---

## 4. What to Observe

During the practical setup, the following observations were verified:

| Observation            | Result                 |
| ---------------------- | ---------------------- |
| Node.js executable     | Located through NVM    |
| Active Node.js version | `v24.19.0`             |
| npm version            | `11.17.0`              |
| Backend dependencies   | Installed successfully |
| Frontend dependencies  | Installed successfully |
| Backend startup        | Successful             |
| Backend port           | `5001`                 |
| MongoDB connection     | Successful             |
| API response           | `HTTP 200 OK`          |
| API body               | `hello`                |
| Frontend startup       | Successful             |
| Frontend port          | `3000`                 |
| Browser rendering      | Successful             |

The frontend produced an existing ESLint warning related to a missing `dispatch` dependency in `Header.js`, but compilation completed successfully.

---

## 5. Common Mistakes

### Running `npm install` from the wrong directory

The project contains separate `package.json` files:

```text
client/package.json
server/package.json
```

Therefore dependencies must be installed from their respective directories.

```bash
cd server
npm install

cd ../client
npm install
```

---

### Starting the application before installing dependencies

Running:

```bash
npm start
```

before installing dependencies can result in missing-module errors.

Correct workflow:

```text
package.json
      |
      v
npm install
      |
      v
node_modules/
      |
      v
npm start
```

---

### Missing `.env`

The backend requires `MONGO_URI`.

The project provides:

```text
server/.env.example
```

A local environment file was created with:

```bash
cp .env.example .env
```

---

### Confusing `localhost` with the Docker hostname

The local environment used:

```text
mongodb://127.0.0.1:27017/BlogApp
```

The Docker Compose configuration uses:

```text
mongodb://mongo:27017/BlogApp
```

`mongo` is intended to resolve through the Docker network. It is not the same as the host's `127.0.0.1`.

---

### Treating warnings as errors

The frontend compiled with a warning:

```text
Compiled with warnings.
```

A warning does not necessarily prevent the application from running.

The application successfully rendered in the browser.

---

### Running `npm audit fix --force` blindly

The project reported dependency vulnerabilities after installation.

Automatic forced upgrades were not performed because they can introduce breaking dependency changes in an existing project.

Dependency security remediation should be evaluated separately.

---

## 6. Real-World Uses

| Skill               | Real-World Use                                      |
| ------------------- | --------------------------------------------------- |
| NVM                 | Managing Node.js versions across projects           |
| npm                 | Installing and maintaining application dependencies |
| `.env`              | Managing environment-specific configuration         |
| `npm start`         | Running development applications                    |
| `curl`              | Testing APIs and health endpoints                   |
| `npm list`          | Investigating installed dependencies                |
| `package.json`      | Defining application dependencies and scripts       |
| `package-lock.json` | Reproducible npm dependency installation            |
| `node_modules`      | Providing locally installed application packages    |

---

## 7. Interview Questions

### Q1. What is Node.js?

Node.js is a JavaScript runtime that allows JavaScript to run outside the browser.

### Q2. What is npm?

npm is the package manager used to install and manage Node.js packages and execute project scripts.

### Q3. Why is NVM useful?

NVM allows developers to install and switch between multiple Node.js versions.

### Q4. What is `package.json`?

It is the project manifest containing metadata, dependencies, development dependencies, and npm scripts.

### Q5. What does `npm install` do?

It installs the dependencies defined by the project's package configuration and creates or updates the dependency lock file.

### Q6. What is `node_modules`?

It contains the packages installed for a Node.js project.

### Q7. Why should `node_modules` normally not be committed to Git?

Because it is generated from the dependency manifest and can be recreated using `npm install`.

### Q8. What is `.env` used for?

It stores environment-specific configuration values such as database connection information.

### Q9. What is the difference between `npm install` and `npm start`?

`npm install` installs dependencies.

`npm start` executes the project's `start` script.

### Q10. Why was `curl` used in this project?

It was used to verify that the backend HTTP endpoint was reachable and returning a successful response.

---

## 8. Configuration Files

| File                  | Location            | Purpose                                    |
| --------------------- | ------------------- | ------------------------------------------ |
| `package.json`        | `server/`           | Backend package configuration              |
| `package-lock.json`   | `server/`           | Backend dependency lock file               |
| `package.json`        | `client/`           | Frontend package configuration             |
| `package-lock.json`   | `client/`           | Frontend dependency lock file              |
| `.env`                | `server/`           | Local backend configuration                |
| `.env.example`        | `server/`           | Environment configuration template         |
| `.gitignore`          | Project directories | Defines files excluded from Git            |
| `docker-compose.yaml` | Project root        | Defines containerized application services |

---

## 9. Practical Project Command Flow

The essential workflow used for this real project can be summarized as:

```text
Clone Repository
      |
      v
Inspect Project
      |
      v
Verify Node.js + npm
      |
      v
npm install
      |
      v
Create .env
      |
      v
npm start
      |
      v
Verify Backend with curl
      |
      v
npm start (client)
      |
      v
Verify Frontend in Browser
```


# Step 15 — Node.js Environment

## Part 3 — Real Lab, Skills and Final Record

---

## 1. Skills Gained

After completing this step, the following practical skills were gained:

* Clone and inspect a real GitHub project.
* Identify frontend and backend components of a MERN application.
* Verify an existing Node.js and npm environment.
* Understand NVM-managed Node.js versions.
* Install project-specific npm dependencies.
* Understand the role of `package.json` and `package-lock.json`.
* Configure a backend `.env` file.
* Start a Node.js and Express backend.
* Verify MongoDB connectivity from the backend.
* Test a backend API from the terminal.
* Start a React frontend.
* Verify the complete application through a web browser.
* Understand the local development workflow of a MERN application.

---

## 2. Real Lab Summary

| Item                      | Actual Lab Result                                           |
| ------------------------- | ----------------------------------------------------------- |
| Practice Project          | `Blog-App-using-MERN-stack`                                 |
| Source                    | GitHub                                                      |
| Project Location          | `~/Projects/mern-devops-practice/Blog-App-using-MERN-stack` |
| Node.js                   | `v24.19.0`                                                  |
| npm                       | `11.17.0`                                                   |
| Node.js Manager           | NVM                                                         |
| Backend Framework         | Express                                                     |
| Backend Port              | `5001`                                                      |
| Frontend                  | React                                                       |
| Frontend Port             | `3000`                                                      |
| Database                  | MongoDB                                                     |
| Database Driver/ODM       | Mongoose                                                    |
| Environment Configuration | `.env`                                                      |
| API Verification          | `curl`                                                      |
| Backend Status            | Successfully running                                        |
| MongoDB Status            | Successfully connected                                      |
| Frontend Status           | Successfully compiled and rendered                          |

---

## 3. Real Machine Information

The Node.js environment used during this lab was:

```text
Node.js : v24.19.0
npm     : 11.17.0
NVM     : Active
Node Path:
/home/afroza/.nvm/versions/node/v24.19.0/bin/node

npm Path:
/home/afroza/.nvm/versions/node/v24.19.0/bin/npm
```

The active Node.js version was managed through NVM.

---

## 4. Project Information

```text
Project:
Blog-App-using-MERN-stack

Project Location:
~/Projects/mern-devops-practice/Blog-App-using-MERN-stack
```

The project contains separate frontend and backend applications:

```text
Blog-App-using-MERN-stack/
├── client/
├── server/
├── .github/
├── docker-compose.yaml
└── README.md
```

---

## 5. Files Used During the Lab

| File                       | Role                                          |
| -------------------------- | --------------------------------------------- |
| `server/package.json`      | Backend dependencies and scripts              |
| `server/package-lock.json` | Backend dependency lock                       |
| `server/.env.example`      | Environment configuration template            |
| `server/.env`              | Local MongoDB configuration                   |
| `server/server.js`         | Backend entry point                           |
| `server/config/db.js`      | MongoDB connection                            |
| `client/package.json`      | Frontend dependencies and scripts             |
| `client/package-lock.json` | Frontend dependency lock                      |
| `docker-compose.yaml`      | Container configuration examined during setup |

---

## 6. Dependencies Installed

### Backend

| Package    | Purpose                              |
| ---------- | ------------------------------------ |
| `express`  | Backend web server                   |
| `mongoose` | MongoDB connection and data modeling |
| `dotenv`   | Environment variable loading         |
| `cors`     | Cross-origin request handling        |
| `helmet`   | HTTP security headers                |
| `bcryptjs` | Password hashing                     |
| `nodemon`  | Development-time automatic restart   |

### Frontend

The React application dependencies were installed using:

```bash
npm install
```

The project uses React, React DOM, Axios, React Router, Redux Toolkit, Material UI and other frontend packages defined in `client/package.json`.

---

## 7. Environment Variables

The backend environment was configured using:

```text
MONGO_URI=mongodb://127.0.0.1:27017/BlogApp
```

The `.env` file allowed the backend to obtain the MongoDB connection string without hard-coding the environment-specific value directly into the application configuration.

---

## 8. Services and Processes Used

| Component                | Role                        | Status During Verification |
| ------------------------ | --------------------------- | -------------------------- |
| Node.js                  | Backend runtime             | Running                    |
| Nodemon                  | Backend development process | Running                    |
| Express                  | HTTP server                 | Running                    |
| MongoDB                  | Database                    | Connected                  |
| React development server | Frontend server             | Running                    |

---

## 9. Verification Results

### Backend Verification

The backend started with:

```text
app started at 5001...
connected!
```

The API was tested using:

```bash
curl -i http://localhost:5001/api
```

The response returned:

```text
HTTP/1.1 200 OK
```

with:

```text
hello
```

This verified that the Express backend was reachable through HTTP.

### Frontend Verification

The React development server successfully compiled and was accessible at:

```text
http://localhost:3000
```

The application rendered in the browser and displayed the Blog App interface.

The frontend generated an existing ESLint warning, but compilation completed successfully.

---

## 10. Project Architecture After Step 15

```text
                    Browser
                       |
                       | HTTP :3000
                       v
               React Frontend
                  client/
                       |
                       | API Requests
                       v
             Node.js + Express
                  server/
                       |
                       | Mongoose
                       v
                  MongoDB
                   :27017
```

This represents the locally verified application flow after completing Step 15.

---

## 11. Important Commands

```bash
git clone
node --version
npm --version
nvm current
nvm ls
npm install
npm list --depth=0
npm start
curl -i http://localhost:5001/api
```

These commands formed the core operational workflow of the step.

---

## 12. Key Takeaways

* Node.js provides the runtime required by the backend.
* npm manages project-specific JavaScript dependencies.
* NVM manages Node.js versions.
* `package.json` defines the project's dependencies and scripts.
* `node_modules` contains installed project dependencies.
* `.env` provides environment-specific configuration.
* Express runs the backend HTTP server.
* Mongoose connects the Node.js application to MongoDB.
* `curl` can verify backend API availability without a browser.
* React runs separately from the Node.js backend during local development.
* A real MERN application consists of multiple cooperating components rather than a single process.

---

## 13. Final Project State

After completing Step 15, the cloned MERN application was successfully prepared for local development.

```text
GitHub Repository
        |
        v
Local Project
        |
        v
Node.js Environment
        |
        v
Dependencies Installed
        |
        v
Environment Configured
        |
        v
Backend Running
        |
        v
MongoDB Connected
        |
        v
API Verified
        |
        v
React Frontend Running
        |
        v
Application Verified in Browser
```

The project is now ready for the next relevant technology integration in the roadmap.

---

## 14. Folder Structure

### Documentation Repository

```text
~/Projects/devops-learning/
└── Step-15-Node.js-Environment/
    └── README.md
```

### Practice Project

```text
~/Projects/mern-devops-practice/
└── Blog-App-using-MERN-stack/
    ├── client/
    ├── server/
    ├── .github/
    ├── docker-compose.yaml
    └── README.md
```

---

## 15. Step Summary

Step 15 established the Node.js environment and applied it to a real MERN application.

The project was cloned from GitHub, inspected, configured, dependencies were installed, the Node.js backend was started, MongoDB connectivity was verified, the backend API was tested, and the React frontend was successfully launched and verified in the browser.

This step established the local application environment that will be used as the foundation for subsequent project-based DevOps work.


