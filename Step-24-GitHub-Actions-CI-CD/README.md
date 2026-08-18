# Step 24 — GitHub Actions (CI/CD)

## Part 1 — GitHub Actions & CI/CD Fundamentals

---

## 1. Overview

GitHub Actions is a CI/CD automation platform built into GitHub.

It allows a repository to automatically perform tasks when specific GitHub events occur.

For example:

    git push
        ↓
    GitHub
        ↓
    GitHub Actions
        ↓
    Install dependencies
        ↓
    Run tests
        ↓
    Build application
        ↓
    Pass / Fail

In this step, GitHub Actions was introduced using a MERN project to understand how application validation can be automated through a CI pipeline.

---

## 2. CI/CD

CI/CD represents two related but different concepts.

| Concept | Full Form | Main Purpose |
|---|---|---|
| CI | Continuous Integration | Automatically validate code changes |
| CD | Continuous Delivery / Deployment | Automatically deliver or deploy validated code |

### CI

CI focuses on checking whether new code is acceptable.

Typical CI tasks:

- Checkout source code
- Install dependencies
- Run tests
- Validate code
- Build application
- Generate build artifacts

Example:

    git push
        ↓
    CI
        ↓
    Install
        ↓
    Test
        ↓
    Build
        ↓
    Pass / Fail

### CD

CD starts after the code has successfully passed the required CI checks.

Typical CD tasks:

- Deploy application
- Publish Docker image
- Release application
- Update server/environment
- Promote a release

General flow:

    CI PASS
       ↓
    CD
       ↓
    Deployment

In this step, the main practical focus was CI. CD deployment was introduced conceptually but not performed as a production deployment.

---

## 3. CI vs CD

| Feature | CI | CD |
|---|---|---|
| Main purpose | Validate code | Deliver/deploy code |
| Tests | Yes | Usually uses CI results |
| Build | Yes | May use CI build output |
| Deployment | No | Yes |
| Failure action | Stop pipeline | Stop deployment |
| Example | npm test, npm run build | Deploy to server/cloud |

---

## 4. GitHub Actions

GitHub Actions executes automation workflows inside GitHub.

A workflow is normally stored inside:

    .github/workflows/

Workflow files use YAML.

Example structure:

    repository/
    ├── .github/
    │   └── workflows/
    │       └── ci.yml
    ├── client/
    ├── server/
    ├── docker-compose.yaml
    └── README.md

The workflow file defines:

- When automation should run
- What runner should be used
- What jobs should run
- What steps each job should execute

---

## 5. Workflow

A workflow is the complete automation definition.

It describes the entire CI/CD process.

Example conceptual workflow:

    Workflow
       │
       ├── Trigger
       │
       └── Jobs
            │
            ├── Client CI
            │
            └── Server CI

A repository can contain multiple workflows for different purposes.

Examples:

- CI workflow
- Deployment workflow
- Release workflow
- Security scanning workflow

---

## 6. Trigger

A trigger defines when a workflow starts.

In our CI workflow, two important triggers were used:

    push
    pull_request

### Push

The workflow runs when code is pushed to the selected branch.

Example:

    on:
      push:
        branches:
          - main

Meaning:

A push to the `main` branch triggers the workflow.

### Pull Request

The workflow can also run when a Pull Request targets the selected branch.

Example:

    on:
      pull_request:
        branches:
          - main

This allows code to be checked before it is merged.

---

## 7. Workflow Trigger Comparison

| Trigger | When it runs | Purpose |
|---|---|---|
| `push` | Code pushed to branch | Validate committed changes |
| `pull_request` | PR targets branch | Validate proposed changes |

Our final CI workflow used both:

    push → main
    pull_request → main

---

## 8. Job

A job is a major unit of work inside a workflow.

A workflow can contain multiple jobs.

Example:

    jobs:
      client-ci:
        ...
      
      server-ci:
        ...

Conceptually:

    Workflow
       │
       ├── Client CI Job
       │
       └── Server CI Job

Each job runs on its own runner environment.

---

## 9. Step

A step is an individual action or command executed inside a job.

Example:

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4

      - name: Install dependencies
        run: npm install

A job contains multiple steps.

Relationship:

    Workflow
        ↓
      Job
        ↓
      Step
        ↓
      Command / Action

---

## 10. Job vs Step

| Job | Step |
|---|---|
| Major unit of work | Individual task |
| Runs on a runner | Runs inside a job |
| Can contain many steps | Performs one specific action |
| Can run independently | Executes in job order |

Example:

    Client CI                 ← Job

        Checkout              ← Step
        Setup Node             ← Step
        Install                ← Step
        Test                   ← Step
        Build                  ← Step

---

## 11. Runner

A runner is the machine/environment where a GitHub Actions job executes.

Our workflow used:

    runs-on: ubuntu-latest

This means GitHub provides an Ubuntu environment for the job.

Conceptually:

    GitHub Actions
          ↓
    Ubuntu Runner
          ↓
    Checkout repository
          ↓
    Install Node.js
          ↓
    Run commands

The runner is temporary for the workflow execution.

---

## 12. `runs-on`

`runs-on` specifies the operating environment for a job.

Example:

    jobs:
      ci:
        runs-on: ubuntu-latest

In our workflow:

    client-ci:
      runs-on: ubuntu-latest

    server-ci:
      runs-on: ubuntu-latest

Therefore both CI jobs execute on GitHub-hosted Ubuntu runners.

---

## 13. `uses` vs `run`

GitHub Actions steps can commonly use either `uses` or `run`.

### `uses`

`uses` executes a reusable GitHub Action.

Example:

    uses: actions/checkout@v4

This uses the official Checkout Action.

Another example:

    uses: actions/setup-node@v4

This uses the Node.js setup action.

### `run`

`run` executes a shell command.

Example:

    run: npm install

Another example:

    run: npm run build

---

## 14. `uses` vs `run` Comparison

| Syntax | Purpose | Example |
|---|---|---|
| `uses` | Reusable Action | `actions/checkout@v4` |
| `run` | Shell command | `npm install` |

Example workflow sequence:

    uses → checkout repository
    uses → setup Node.js
    run  → npm install
    run  → npm test
    run  → npm run build

---

## 15. Basic GitHub Actions Architecture

The concepts learned in this part can be connected as:

    Git Event
        ↓
    Workflow Trigger
        ↓
    Workflow
        ↓
    Job
        ↓
    Runner
        ↓
    Steps
        ↓
    Commands / Actions
        ↓
    Result
        ↓
    PASS / FAIL

---

## 16. Step 24 Learning Goal

The main goal of Step 24 is to understand how code changes can automatically trigger validation.

The basic DevOps workflow is:

    Developer
        ↓
    git push
        ↓
    GitHub
        ↓
    GitHub Actions
        ↓
    CI Pipeline
        ↓
    Install
        ↓
    Test
        ↓
    Build
        ↓
    PASS / FAIL

If CI fails:

    CI
     ↓
    FAIL
     ↓
    Stop / investigate logs

If CI succeeds:

    CI
     ↓
    PASS
     ↓
    Ready for next stage
     ↓
    CD / Deployment

---

## 17. Key Takeaways

| Concept | What I learned |
|---|---|
| GitHub Actions | GitHub-based automation platform |
| CI | Automatically validates code |
| CD | Delivers/deploys validated code |
| Workflow | Complete automation definition |
| Trigger | Determines when workflow starts |
| Job | Major unit of work |
| Step | Individual task inside a job |
| Runner | Environment where job executes |
| `runs-on` | Selects runner environment |
| `uses` | Executes reusable GitHub Action |
| `run` | Executes shell command |
| `push` | Can trigger CI after code push |
| `pull_request` | Can validate proposed changes |

---

## 18. Part 1 Summary

GitHub Actions provides an automated environment where repository events can trigger predefined workflows.

The basic hierarchy is:

    Trigger
       ↓
    Workflow
       ↓
    Job
       ↓
    Runner
       ↓
    Steps
       ↓
    Actions / Commands
       ↓
    Result

This foundation was used in the following parts to build and understand a real MERN CI pipeline.

# Step 24 — GitHub Actions (CI/CD)

## Part 2 — Actual MERN CI Workflow

---

## 1. Practical Goal

Part 1-এ GitHub Actions-এর basic concepts শেখার পর এই part-এ একটি real MERN project-এর জন্য CI workflow তৈরি করা হয়েছে।

Project structure:

    Blog-App-using-MERN-stack/
    ├── client/
    ├── server/
    ├── docker-compose.yaml
    ├── .github/
    │   └── workflows/
    │       └── ci.yml
    └── README.md

The CI pipeline validates the frontend and backend separately.

---

## 2. Project CI Flow

The MERN project contains two main application parts:

    Client
      ↓
    React application

    Server
      ↓
    Node.js / Express application

Therefore the CI workflow was divided into two jobs:

    MERN CI
       │
       ├── Client CI
       │
       └── Server CI

This makes frontend and backend validation easier to understand and troubleshoot.

---

## 3. CI Trigger

The workflow was configured to run for:

    push → main

and:

    pull_request → main

Workflow section:

    on:
      push:
        branches:
          - main
      pull_request:
        branches:
          - main

### Meaning

| Trigger | Meaning |
|---|---|
| `push` | Run CI when changes are pushed to `main` |
| `pull_request` | Run CI when a PR targets `main` |

This prevents CI from being limited to only local testing.

---

## 4. Client CI Job

The frontend job was defined as:

    client-ci:
      name: Client CI
      runs-on: ubuntu-latest

The job runs on a GitHub-hosted Ubuntu runner.

Its workflow is:

    Checkout
       ↓
    Setup Node.js
       ↓
    Install dependencies
       ↓
    Run tests
       ↓
    Build client

---

## 5. Checkout Repository

The first client step:

    - name: Checkout repository
      uses: actions/checkout@v4

### Purpose

The GitHub Actions runner starts with a clean environment.

The repository source code must therefore be downloaded into the runner before other commands can access:

- `client/`
- `server/`
- configuration files
- workflow-related project files

`actions/checkout@v4` performs this repository checkout.

---

## 6. Setup Node.js

The workflow then configures Node.js:

    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 20

### Purpose

The CI runner needs the correct Node.js environment before executing npm commands.

The workflow uses:

    node-version: 20

Therefore the client CI job runs its Node.js commands using Node.js 20.

---

## 7. `working-directory`

The client commands use:

    working-directory: ./client

Example:

    - name: Install client dependencies
      working-directory: ./client
      run: npm install

This means the command executes from:

    project-root/client

instead of the repository root.

### Why this matters

The client has its own `package.json`.

Therefore:

    npm install

must operate against:

    ./client/package.json

rather than the project root.

---

## 8. Client Dependency Installation

The client dependencies are installed using:

    - name: Install client dependencies
      working-directory: ./client
      run: npm install

Workflow:

    client/
       ↓
    package.json
       ↓
    npm install
       ↓
    node_modules

This prepares the React application for testing and production build.

---

## 9. Client Tests

The client test step:

    - name: Run client tests
      working-directory: ./client
      run: CI=true npm test -- --watchAll=false

Two important parts are used here:

    CI=true

and:

    --watchAll=false

### `CI=true`

This tells the test environment that the command is running in a CI environment.

### `--watchAll=false`

This prevents Jest from continuously waiting for file changes.

CI requires a non-interactive test execution.

Therefore the test command can finish with a clear success or failure result.

---

## 10. Client Build

After testing:

    - name: Build client
      working-directory: ./client
      run: npm run build

The React application is compiled into a production build.

Workflow:

    Source Code
        ↓
    npm run build
        ↓
    Production Build
        ↓
    client/build/

A successful build means the frontend can be compiled into deployable production assets.

---

## 11. Server CI Job

The backend was separated into another job:

    server-ci:
      name: Server CI
      runs-on: ubuntu-latest

Its workflow is:

    Checkout
       ↓
    Setup Node.js
       ↓
    Install dependencies
       ↓
    Validate server.js

---

## 12. Server Checkout

The server job also starts with:

    - name: Checkout repository
      uses: actions/checkout@v4

Each job runs in its own runner environment.

Therefore the server job must also checkout the repository.

---

## 13. Server Node.js Setup

The server uses the same Node.js setup:

    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 20

This keeps the CI Node.js environment consistent.

---

## 14. Server Dependency Installation

The server dependencies are installed with:

    - name: Install server dependencies
      working-directory: ./server
      run: npm install

Again, `working-directory` ensures that npm uses:

    server/package.json

instead of looking at the repository root.

---

## 15. Server Validation

The server validation step:

    - name: Validate server
      working-directory: ./server
      run: node --check server.js

The command:

    node --check server.js

checks the JavaScript syntax of `server.js`.

It does not start the complete application.

It is a lightweight syntax validation step.

### Result

    Valid JavaScript
        ↓
    Exit code 0
        ↓
    CI step passes

    Syntax error
        ↓
    Non-zero exit code
        ↓
    CI step fails

---

## 16. Client CI vs Server CI

| Feature | Client CI | Server CI |
|---|---|---|
| Job name | `client-ci` | `server-ci` |
| Runner | `ubuntu-latest` | `ubuntu-latest` |
| Checkout | Yes | Yes |
| Node.js | Node 20 | Node 20 |
| Working directory | `./client` | `./server` |
| Install | `npm install` | `npm install` |
| Test | Yes | No dedicated test |
| Build | `npm run build` | No |
| Validation | React test/build | `node --check server.js` |

---

## 17. Why Separate Jobs?

The frontend and backend have different responsibilities.

Client:

    React
      ↓
    Test
      ↓
    Build

Server:

    Node.js / Express
      ↓
    Dependency install
      ↓
    JavaScript validation

Separating them provides clearer CI results.

For example:

    Client CI → FAIL

does not mean:

    Server CI → FAIL

The jobs have independent execution environments.

---

## 18. Complete 24.4 CI Structure

The practical CI structure became:

    MERN CI
       │
       ├── Client CI
       │    │
       │    ├── Checkout
       │    ├── Node 20
       │    ├── npm install
       │    ├── npm test
       │    └── npm run build
       │
       └── Server CI
            │
            ├── Checkout
            ├── Node 20
            ├── npm install
            └── node --check server.js

---

## 19. Final Workflow Used Before 24.6

The main MERN CI workflow structure was:

    name: MERN CI

    on:
      push:
        branches:
          - main
      pull_request:
        branches:
          - main

    jobs:
      client-ci:
        name: Client CI
        runs-on: ubuntu-latest

        steps:
          - name: Checkout repository
            uses: actions/checkout@v4

          - name: Setup Node.js
            uses: actions/setup-node@v4
            with:
              node-version: 20

          - name: Install client dependencies
            working-directory: ./client
            run: npm install

          - name: Run client tests
            working-directory: ./client
            run: CI=true npm test -- --watchAll=false

          - name: Build client
            working-directory: ./client
            run: npm run build

      server-ci:
        name: Server CI
        runs-on: ubuntu-latest

        steps:
          - name: Checkout repository
            uses: actions/checkout@v4

          - name: Setup Node.js
            uses: actions/setup-node@v4
            with:
              node-version: 20

          - name: Install server dependencies
            working-directory: ./server
            run: npm install

          - name: Validate server
            working-directory: ./server
            run: node --check server.js

---

## 20. `needs` — Job Dependency

In the final Step 24 workflow, Docker build was connected to the CI jobs using:

    needs:
      - client-ci
      - server-ci

This creates the dependency:

    Client CI ──────┐
                    ├──→ Docker Build
    Server CI ──────┘

The Docker build job waits until both required CI jobs complete successfully.

### Meaning

    client-ci PASS
            +
    server-ci PASS
            ↓
    docker-build starts

If a required job fails:

    client-ci FAIL
            ↓
    docker-build does not proceed

---

## 21. Why `needs` Is Important

Without `needs`, jobs can execute independently.

With:

    needs:
      - client-ci
      - server-ci

we explicitly define that Docker build depends on successful CI validation.

This creates a safer pipeline:

    Validate application
          ↓
    CI PASS
          ↓
    Build Docker images

instead of:

    Docker build
          ↓
    Discover problems later

---

## 22. Step Execution vs Job Dependency

These are different concepts.

| Concept | Controls |
|---|---|
| Step order | Order inside one job |
| `needs` | Dependency between jobs |

Example:

    Client CI Job

    Checkout
       ↓
    Install
       ↓
    Test
       ↓
    Build

Here normal step order controls execution.

But:

    client-ci
         +
    server-ci
         ↓
    docker-build

Here `needs` controls job dependency.

---

## 23. Actual MERN CI Learning

The practical pipeline taught the following sequence:

    Code
      ↓
    Git push / Pull Request
      ↓
    GitHub Actions
      ↓
    Client CI
      ├── Install
      ├── Test
      └── Build
      ↓
    Server CI
      ├── Install
      └── Validate
      ↓
    Successful CI
      ↓
    Docker Build

This connects the GitHub Actions concepts from Part 1 with the Docker and Docker Compose knowledge from Steps 22 and 23.

---

## 24. Important Commands Used in CI

| Command | Purpose |
|---|---|
| `npm install` | Install project dependencies |
| `npm test` | Run application tests |
| `npm run build` | Create production frontend build |
| `node --check server.js` | Validate JavaScript syntax |
| `docker build` | Build Docker image |

---

## 25. Important GitHub Actions Configuration

| Configuration | Purpose |
|---|---|
| `name:` | Workflow/job/step name |
| `on:` | Workflow trigger |
| `push:` | Trigger on push |
| `pull_request:` | Trigger on pull request |
| `jobs:` | Defines jobs |
| `runs-on:` | Selects runner |
| `steps:` | Defines job steps |
| `uses:` | Uses reusable GitHub Action |
| `run:` | Executes shell command |
| `with:` | Provides action configuration |
| `working-directory:` | Changes command execution directory |
| `needs:` | Defines job dependency |

---

## 26. Part 2 Summary

The first practical MERN CI pipeline was built using GitHub Actions.

The workflow validates the project through separate frontend and backend jobs.

Final basic flow:

    Push / Pull Request
          ↓
    GitHub Actions
          ↓
    ┌─────────────────┐
    │    Client CI    │
    │ Install         │
    │ Test            │
    │ Build           │
    └─────────────────┘
          +
    ┌─────────────────┐
    │    Server CI    │
    │ Install         │
    │ Validate        │
    └─────────────────┘
          ↓
    CI Result
          ↓
    Docker Build

The workflow demonstrates how GitHub Actions can automatically validate a multi-part MERN application before moving toward container build and later deployment stages.

# Step 24 — GitHub Actions (CI/CD)

## Part 3 — Practical Observations, Problems & Debugging

---

## 1. Practical CI Validation

The CI pipeline was tested locally before relying on GitHub Actions.

The main validation flow was:

    Client
      ↓
    npm install
      ↓
    npm test
      ↓
    npm run build

    Server
      ↓
    npm install
      ↓
    node --check server.js

This helped identify the difference between:

- dependency installation
- testing
- production build
- syntax validation

---

## 2. Client Dependency Installation

The client dependency installation completed successfully.

Observed result:

    npm install
    ↓
    packages installed
    ↓
    audit report generated

However, npm reported dependency vulnerabilities.

Observed:

    31 vulnerabilities
    9 low
    7 moderate
    15 high

npm also displayed:

    npm audit fix
    npm audit fix --force

### Important observation

A vulnerability report does not automatically mean that the CI installation failed.

The installation can succeed while npm still reports security vulnerabilities.

Therefore:

    npm install → SUCCESS
    security audit → warnings / vulnerabilities

These are different results.

---

## 3. npm Vulnerability Reports

The client installation reported:

    31 vulnerabilities
    9 low
    7 moderate
    15 high

The server installation later reported:

    35 vulnerabilities
    12 low
    10 moderate
    11 high
    2 critical

### Observation

The CI pipeline should distinguish between:

- installation success
- application test result
- build result
- security audit result

A vulnerability count should not automatically be treated as a build failure unless the workflow explicitly enforces a security policy.

---

## 4. npm Install Script Warning

During client dependency installation, npm also reported:

    npm warn allow-scripts

It identified packages with install scripts that had not yet been approved.

The output mentioned:

    core-js
    core-js-pure

### Observation

This was an npm package-installation warning.

It was not the reason the client build failed or succeeded.

Therefore:

    Warning ≠ CI failure

The warning should be investigated separately when hardening the dependency supply chain.

---

## 5. npm Version Notice

npm also reported that a newer major npm version was available.

Observed:

    npm 11.17.0
    ↓
    npm 12.0.2 available

### Observation

This was only a version update notice.

It did not indicate that the project was broken.

Therefore:

    New npm version available
              ≠
          CI failure

No global npm upgrade was performed as part of this CI learning step.

---

## 6. Client Build Result

The React production build completed successfully.

Observed:

    Creating an optimized production build...
    Compiled with warnings.
    ...
    The build folder is ready to be deployed.

Therefore:

    Client Build → PASS

The build generated production assets under:

    client/build/

### Important distinction

The build produced a warning related to React Hook dependencies:

    React Hook useEffect has a missing dependency: 'dispatch'

This warning did not prevent the production build from completing.

Therefore:

    Build → PASS
    ESLint warning → PRESENT

---

## 7. Client Test Failure

The client test did not pass.

The failing test was:

    renders learn react link

The important error was:

    could not find react-redux context value

The error occurred because the application uses Redux functionality while the test renders the application without the required Redux context.

The relevant relationship was:

    App.test.js
        ↓
    render(<App />)
        ↓
    App.js
        ↓
    useDispatch()
        ↓
    Redux context unavailable
        ↓
    Test FAIL

---

## 8. Root Cause vs Warning

During debugging, it was important to distinguish the actual failure from unrelated warnings.

| Output | Classification | Effect |
|---|---|---|
| `could not find react-redux context value` | Actual error | Test fails |
| `useEffect` missing dependency warning | Warning | Build still succeeds |
| npm vulnerabilities | Security warning/report | Install can still succeed |
| npm update notice | Informational | No CI failure |
| npm allow-scripts warning | Warning | Installation can still succeed |

This is an important CI debugging habit:

    Do not treat every red/yellow message as the root cause.

Find the message that actually causes the command to exit unsuccessfully.

---

## 9. Why the Client CI Failed

The client CI contained:

    npm test

followed by:

    npm run build

Because the test failed, the workflow execution would stop at the test step.

Therefore:

    Install       → PASS
    Test          → FAIL
    Build         → NOT REACHED

This demonstrates normal CI fail-fast behavior inside a job.

---

## 10. Client Test vs Client Build

An important observation from the project was:

    Client Test → FAIL
    Client Build → PASS

These are not contradictory.

Testing asks:

    "Does the application pass the defined tests?"

Building asks:

    "Can the application be compiled into a production build?"

One can succeed while the other fails.

| Operation | Result |
|---|---|
| Client dependency installation | PASS |
| Client tests | FAIL |
| Client production build | PASS when run separately |

---

## 11. Server Dependency Installation

The server dependency installation completed successfully.

Observed:

    up to date
    audited 222 packages

However, npm reported:

    35 vulnerabilities
    12 low
    10 moderate
    11 high
    2 critical

Again:

    npm install → SUCCESS
    vulnerability report → PRESENT

The vulnerabilities were not automatically treated as a server syntax failure.

---

## 12. Server Validation

The server was validated using:

    node --check server.js

The command completed without an error.

Therefore:

    Server syntax validation → PASS

This was used because the project's current test script intentionally exits with failure:

    "test": "echo \"Error: no test specified\" && exit 1"

Therefore the CI workflow did not use the current server `npm test` script as the backend validation gate.

---

## 13. Why Server `npm test` Was Not Used

The server package contains:

    "test": "echo \"Error: no test specified\" && exit 1"

Running this script would intentionally produce a failure.

Therefore:

    npm test
        ↓
    exit 1
        ↓
    CI FAIL

For this learning stage, the workflow instead used:

    node --check server.js

This provides a meaningful syntax validation without pretending that the project already has a real backend test suite.

---

## 14. GitHub Actions Workflow Debugging

The CI debugging process followed this sequence:

    Workflow starts
          ↓
    Identify failed Job
          ↓
    Identify failed Step
          ↓
    Read actual error
          ↓
    Separate warning from error
          ↓
    Identify root cause
          ↓
    Decide whether the problem belongs to
    DevOps workflow or application code

This distinction is important.

Not every application problem needs to be fixed during DevOps training.

---

## 15. Existing Workflow Observation

Before creating the final CI workflow, the project already contained:

    .github/workflows/npm-publish-github-packages.yml

That workflow was designed around package publishing after a GitHub release.

It was not the CI workflow required for this learning step.

Therefore a separate workflow was created:

    .github/workflows/ci.yml

The CI workflow focuses on:

- client validation
- server validation
- build
- Docker integration

---

## 16. `.gitignore` and Environment Secrets

The project contained environment files including:

    server/.env
    server/.env.example

The `.gitignore` was updated to include environment files:

    .env
    .env.*
    !.env.example
    server/.env
    client/.env

### Purpose

Actual environment values should remain local and should not be committed to Git.

The example file can remain available as documentation/template.

Therefore:

    server/.env
        → ignored

    server/.env.example
        → allowed

---

## 17. `.env` vs `.env.example`

| File | Purpose | Git |
|---|---|---|
| `.env` | Actual local secrets/configuration | Ignore |
| `.env.example` | Example variable names/template | Can commit |

Example concept:

    .env

    MONGO_URI=<real-value>

while:

    .env.example

    MONGO_URI=

The real value should never be hard-coded into the workflow.

---

## 18. GitHub Actions Secrets

For CI/CD systems, sensitive values should be stored as GitHub repository/environment secrets instead of being written directly into workflow files.

Conceptual flow:

    Local / GitHub configuration
              ↓
        GitHub Secret
              ↓
    ${{ secrets.SECRET_NAME }}
              ↓
        Workflow step

Examples of values that may require secrets:

- database credentials
- API keys
- deployment credentials
- tokens
- cloud provider credentials

No real secret value was added to the workflow during this learning step.

---

## 19. Why Secrets Should Not Be Hard-Coded

Bad practice:

    run: some-command --password=my-real-password

Better approach:

    run: some-command --password="${{ secrets.PASSWORD }}"

The workflow file may be visible to repository collaborators.

Therefore sensitive credentials should not be written directly into YAML.

---

## 20. Package Lockfiles and npm Cache

The project was checked for lockfiles.

Observed:

    client/package-lock.json
    server/package-lock.json
    server/yarn.lock

Therefore both client and server have npm lockfiles available locally.

The final workflow introduced npm caching through:

    cache: npm

and:

    cache-dependency-path

This allows `setup-node` to use dependency information from the specified lockfile.

---

## 21. Why Caching Matters

Without dependency caching:

    CI run
       ↓
    Download dependencies
       ↓
    Install
       ↓
    Finish

With caching:

    First run
       ↓
    Download dependencies
       ↓
    Cache

    Later run
       ↓
    Reuse cache when possible
       ↓
    Faster dependency installation

Caching improves CI performance but does not replace dependency installation.

---

## 22. Artifact

The final workflow introduced a client build artifact.

Configuration:

    uses: actions/upload-artifact@v4

with:

    name: client-build
    path: client/build

The purpose is to preserve the generated build output after the job finishes.

Conceptual flow:

    npm run build
          ↓
    client/build/
          ↓
    upload-artifact
          ↓
    GitHub Actions Artifact

---

## 23. Artifact vs Docker Image

These are different concepts.

| Artifact | Docker Image |
|---|---|
| Stores generated build/output | Packages application into container |
| Example: `client/build` | Example: `blog-client:ci` |
| Useful for later workflow stages | Useful for container execution/deployment |
| Uploaded by Actions | Built using Docker |

Therefore:

    Build Artifact ≠ Docker Image

---

## 24. Docker Integration

The final workflow introduced a Docker build job.

The Docker build job depends on:

    client-ci
    server-ci

through:

    needs:
      - client-ci
      - server-ci

Conceptually:

    Client CI ──────┐
                    │
                    ├──→ Docker Build
                    │
    Server CI ──────┘

This prevents the Docker build stage from running when required CI validation has failed.

---

## 25. Actual Docker Build Steps

The final workflow used:

    docker build -t blog-client:ci ./client

and:

    docker build -t blog-server:ci ./server

The `-t` flag assigns a tag to the generated image.

### Meaning

    -t blog-client:ci

means:

    image name → blog-client
    tag        → ci

Similarly:

    -t blog-server:ci

creates the server image with the `ci` tag.

---

## 26. `needs` and Failure Propagation

The Docker job was configured with:

    needs:
      - client-ci
      - server-ci

Therefore:

    Client CI PASS
          +
    Server CI PASS
          ↓
    Docker Build

But:

    Client CI FAIL
          ↓
    Docker Build skipped

or:

    Server CI FAIL
          ↓
    Docker Build skipped

This creates a quality gate before container image creation.

---

## 27. Practical Problem Summary

| Problem / Observation | Result | Root Cause / Meaning | Action |
|---|---|---|---|
| Client test failed | ❌ | Redux context unavailable in test | Observed; application test not fixed |
| Client build warning | ⚠️ | Missing `dispatch` dependency warning | Build still succeeded |
| Client vulnerabilities | ⚠️ | npm dependency audit findings | Not treated as CI failure |
| Server vulnerabilities | ⚠️ | npm dependency audit findings | Not treated as syntax failure |
| npm allow-scripts warning | ⚠️ | Package install-script approval warning | Observed |
| npm version notice | ℹ️ | New npm major version available | No upgrade performed |
| Server `npm test` | ❌ | Existing script intentionally exits `1` | Used `node --check` instead |
| `.env` files | ⚠️ | Sensitive configuration exists | Added to `.gitignore` |
| Existing package workflow | ℹ️ | Release/package publishing workflow | Separate CI workflow created |
| Docker build dependency | ✅ | CI jobs must pass first | Used `needs` |

---

## 28. Important Debugging Lesson

The most important lesson from the practical CI failure was:

    Not every warning is a failure.

A useful debugging hierarchy is:

    1. Which Job failed?
    2. Which Step failed?
    3. What command was running?
    4. What was the actual error?
    5. What caused the command to exit non-zero?
    6. Is the problem application code or CI configuration?

This prevents unnecessary changes to a project when the actual problem is unrelated to the DevOps workflow.

---

## 29. Practical Result

The practical results can be summarized as:

    Client dependency installation → PASS
    Client test                 → FAIL
    Client build                → PASS when run separately

    Server dependency installation → PASS
    Server syntax validation       → PASS

    Environment protection         → CONFIGURED
    npm cache concept              → INTRODUCED
    Build artifact                 → INTRODUCED
    Docker build dependency        → CONFIGURED

---

## 30. Part 3 Summary

The practical CI work demonstrated that a CI pipeline produces more than a simple pass/fail result.

It can expose:

- application test failures
- build warnings
- dependency vulnerabilities
- package manager warnings
- syntax errors
- environment configuration issues
- artifact generation
- Docker build dependencies

The most important debugging principle learned was:

    Identify the actual failing command
    before attempting to fix anything.

The CI pipeline should validate the application, while application-specific problems should be fixed separately when appropriate.

# Step 24 — GitHub Actions (CI/CD)

## Part 4 — Final Workflow, Commands & Step Summary

---

## 1. Final Step 24 Architecture

The complete learning flow of Step 24 is:

    Developer
        ↓
    git push / Pull Request
        ↓
    GitHub
        ↓
    GitHub Actions
        ↓
    ┌──────────────────────────────┐
    │          Client CI           │
    │                              │
    │  Checkout                    │
    │      ↓                       │
    │  Node.js 20                  │
    │      ↓                       │
    │  npm install                 │
    │      ↓                       │
    │  npm test                    │
    │      ↓                       │
    │  npm run build               │
    └──────────────┬───────────────┘
                   │
    ┌──────────────▼───────────────┐
    │          Server CI           │
    │                              │
    │  Checkout                    │
    │      ↓                       │
    │  Node.js 20                  │
    │      ↓                       │
    │  npm install                 │
    │      ↓                       │
    │  node --check server.js      │
    └──────────────┬───────────────┘
                   │
                   ▼
             CI Validation
                   │
             ┌─────┴─────┐
             ↓           ↓
           FAIL          PASS
             ↓           ↓
           Stop       Docker Build
                         ↓
                    Build Artifact
                         ↓
                    Future CD
                         ↓
                     Deployment

---

## 2. Final Workflow Concept

The final workflow connects application validation with Docker image creation.

The important dependency is:

    client-ci
          +
    server-ci
          ↓
    docker-build

This dependency was implemented using:

    needs:
      - client-ci
      - server-ci

Therefore Docker image building is treated as a later stage after required CI validation.

---

## 3. Final Workflow Structure

The final workflow contains three jobs:

    jobs:
      client-ci:
        ...

      server-ci:
        ...

      docker-build:
        needs:
          - client-ci
          - server-ci
        ...

### Job responsibilities

| Job | Responsibility |
|---|---|
| `client-ci` | Install, test and build React client |
| `server-ci` | Install and validate Node.js server |
| `docker-build` | Build client and server Docker images after CI passes |

---

## 4. Final Client CI Flow

    client-ci
       ↓
    Checkout repository
       ↓
    Setup Node.js 20
       ↓
    npm install
       ↓
    npm test
       ↓
    npm run build
       ↓
    Upload client build artifact

The client build artifact is generated from:

    client/build/

and uploaded using:

    actions/upload-artifact@v4

---

## 5. Final Server CI Flow

    server-ci
       ↓
    Checkout repository
       ↓
    Setup Node.js 20
       ↓
    npm install
       ↓
    node --check server.js
       ↓
    Server validation complete

The current project does not have a real backend test suite.

Its existing test script intentionally exits with failure:

    "test": "echo \"Error: no test specified\" && exit 1"

Therefore server CI uses:

    node --check server.js

as the current lightweight validation step.

---

## 6. Final Docker Build Flow

The Docker build job uses:

    needs:
      - client-ci
      - server-ci

After both required jobs succeed:

    docker build -t blog-client:ci ./client

and:

    docker build -t blog-server:ci ./server

are executed.

The resulting images are:

    blog-client:ci
    blog-server:ci

---

## 7. Important Docker Command

### `docker build`

Used to create a Docker image from a Dockerfile and build context.

Example:

    docker build -t blog-client:ci ./client

Structure:

    docker build [OPTIONS] IMAGE_CONTEXT

Important option:

    -t

Meaning:

    --tag

It assigns a name and tag to the resulting image.

Example:

    -t blog-client:ci

means:

    image name = blog-client
    tag        = ci

---

## 8. Important GitHub Actions Commands / Configuration

| Configuration | Purpose |
|---|---|
| `name:` | Names workflow/job/step |
| `on:` | Defines workflow triggers |
| `push:` | Runs workflow after push |
| `pull_request:` | Runs workflow for PR |
| `branches:` | Limits trigger to selected branch |
| `jobs:` | Defines workflow jobs |
| `runs-on:` | Selects runner |
| `steps:` | Defines job steps |
| `uses:` | Uses reusable Action |
| `run:` | Executes shell command |
| `with:` | Provides Action configuration |
| `working-directory:` | Executes command from selected directory |
| `needs:` | Creates job dependency |
| `if:` | Controls conditional execution |
| `cache:` | Enables dependency caching |
| `cache-dependency-path:` | Identifies dependency lockfile |
| `path:` | Specifies artifact path |

---

## 9. Important Shell / Node Commands

| Command | Purpose |
|---|---|
| `npm install` | Install dependencies |
| `npm test` | Run tests |
| `npm run build` | Build frontend |
| `node --check server.js` | Check JavaScript syntax |
| `docker build` | Build Docker image |
| `git push` | Send local commit to remote repository |

---

## 10. Important Command Flags

### npm test

The client CI used:

    CI=true npm test -- --watchAll=false

| Part | Meaning |
|---|---|
| `CI=true` | Run in CI environment mode |
| `--` | Pass remaining arguments to the test command |
| `--watchAll=false` | Disable continuous watch mode |

This is important because CI must finish automatically instead of waiting for file changes.

---

## 11. Node Syntax Validation

The server validation used:

    node --check server.js

`--check` validates JavaScript syntax without executing the application normally.

Therefore it can catch syntax errors such as:

- invalid JavaScript syntax
- malformed statements
- syntax-related parsing errors

It does not replace a complete backend test suite.

---

## 12. Dependency Caching

The final workflow introduced npm caching through:

    cache: npm

and:

    cache-dependency-path: client/package-lock.json

or:

    cache-dependency-path: server/package-lock.json

The purpose is to make repeated CI runs faster by reusing cached npm dependency data when possible.

General flow:

    First CI run
        ↓
    Download dependencies
        ↓
    Cache

    Later CI run
        ↓
    Reuse cache
        ↓
    Faster installation

Caching improves performance but does not replace dependency installation.

---

## 13. Build Artifacts

The client build was uploaded using:

    actions/upload-artifact@v4

Configuration:

    name: client-build
    path: client/build

Purpose:

    Source
      ↓
    npm run build
      ↓
    client/build/
      ↓
    GitHub Actions Artifact

An artifact preserves generated output after the job finishes.

---

## 14. Artifact vs Image

| Artifact | Docker Image |
|---|---|
| Stores generated output | Packages application into container |
| Example: `client/build` | Example: `blog-client:ci` |
| Produced by build process | Produced by Docker |
| Can be uploaded/downloaded | Can be run by Docker |
| Useful for later workflow stages | Useful for container deployment |

Therefore:

    Artifact ≠ Docker Image

---

## 15. Secrets

Sensitive configuration should not be hard-coded into workflow YAML.

Examples:

- Database credentials
- API keys
- Authentication tokens
- Cloud credentials
- Deployment credentials

Conceptual pattern:

    GitHub Secret
         ↓
    ${{ secrets.SECRET_NAME }}
         ↓
    Workflow

Environment files such as:

    server/.env

were kept out of Git using `.gitignore`.

An example configuration file can be maintained as:

    server/.env.example

without exposing the real secret values.

---

## 16. Environment File Comparison

| File | Purpose | Should contain real secret? |
|---|---|---|
| `.env` | Local configuration | Yes, locally if required |
| `.env.example` | Configuration template | No |
| GitHub Secret | CI/CD sensitive configuration | Secure value |

The real `.env` should not be committed to a repository.

---

## 17. Branch Control

The workflow was configured for:

    push:
      branches:
        - main

and:

    pull_request:
      branches:
        - main

This means the workflow is specifically associated with changes involving the `main` branch.

Branch-based CI control prevents every possible branch/event from automatically triggering the same workflow.

---

## 18. CI Failure Behavior

Within a job, steps normally execute sequentially.

Example:

    Install
       ↓
    Test
       ↓
    Build

If the test step fails:

    Install → PASS
    Test    → FAIL
    Build   → STOPPED

This provides a quality gate.

The important principle is:

    Fix the failed stage before continuing.

---

## 19. Job Dependency with `needs`

Jobs can have dependencies.

Example:

    docker-build:
      needs:
        - client-ci
        - server-ci

This creates:

    Client CI ──────┐
                    │
                    ▼
                Docker Build
                    ▲
                    │
    Server CI ──────┘

Docker build starts only after the required CI jobs successfully complete.

---

## 20. CI Failure Debugging Workflow

The debugging process learned in Step 24:

    CI fails
       ↓
    Identify failed Job
       ↓
    Identify failed Step
       ↓
    Identify command
       ↓
    Read actual error
       ↓
    Separate warning from error
       ↓
    Find root cause
       ↓
    Decide whether it is:
       ├── CI configuration issue
       └── Application issue
       ↓
    Fix appropriate layer

This prevents unnecessary modifications.

---

## 21. Important Practical Lesson

During this step, the downloaded MERN project produced several warnings and one actual test failure.

The important distinction was:

    Warning
       ≠
    Failure

Examples:

    npm vulnerabilities
    npm version notice
    React Hook warning
    npm allow-scripts warning

were different from the actual test error:

    could not find react-redux context value

Therefore CI debugging should focus first on the command that actually failed.

---

## 22. CI vs CD Final Architecture

The final conceptual DevOps pipeline is:

    Developer
        ↓
    git push
        ↓
    GitHub
        ↓
    CI
        ├── Install
        ├── Test
        ├── Build
        └── Validate
        ↓
    PASS
        ↓
    Docker Build
        ↓
    Artifact / Image
        ↓
    CD
        ↓
    Deployment

CI answers:

    "Is this code valid enough to continue?"

CD answers:

    "How do we deliver/deploy the validated code?"

---

## 23. What Was Practically Completed

| Area | Status |
|---|---|
| GitHub Actions fundamentals | Completed |
| Workflow triggers | Completed |
| Jobs and steps | Completed |
| GitHub-hosted runner | Completed |
| Node.js setup | Completed |
| Client dependency installation | Completed |
| Client testing | Completed |
| Client production build | Completed |
| Server dependency installation | Completed |
| Server syntax validation | Completed |
| CI failure investigation | Completed |
| `.env` protection | Completed |
| GitHub Secrets concept | Introduced |
| npm caching concept | Introduced |
| Build artifacts | Introduced |
| Docker build integration | Completed |
| `needs` dependency | Completed |
| CI vs CD architecture | Completed |

---

## 24. What Was Not Done

The following were intentionally not performed in this step:

- Production cloud deployment
- Real CD deployment
- Real production secrets
- Docker image push to a registry
- Production release automation

These require a later deployment stage and are outside the practical scope of Step 24.

---

## 25. Step 24 Lessons Learned

### Lesson 1 — CI is automated validation

Instead of manually checking every change:

    Developer
       ↓
    Push
       ↓
    Automated validation

This makes development feedback faster and repeatable.

### Lesson 2 — Jobs should represent meaningful units

Frontend and backend were separated into:

    Client CI
    Server CI

This makes failures easier to locate.

### Lesson 3 — Failure should stop unsafe progression

Docker build depends on successful CI:

    CI PASS
       ↓
    Docker Build

This prevents known-invalid code from automatically progressing.

### Lesson 4 — Logs are essential

A CI system is useful only if failures can be diagnosed from its logs.

### Lesson 5 — Warnings and errors are different

A professional DevOps workflow does not blindly treat every warning as a failure.

### Lesson 6 — Secrets must be separated from source code

Credentials belong in secure secret storage, not inside workflow files.

### Lesson 7 — CI and CD are different stages

CI validates the code.

CD delivers or deploys the validated code.

---

## 26. Final Step 24 Flow

The complete Step 24 learning flow:

    Git
      ↓
    GitHub
      ↓
    GitHub Actions
      ↓
    Workflow Trigger
      ↓
    Runner
      ↓
    Client CI ──→ Install → Test → Build
      │
      └──────────────┐
                     ↓
    Server CI ──→ Install → Validate
                     ↓
                 CI Result
                     ↓
               ┌─────┴─────┐
               ↓           ↓
             FAIL         PASS
               ↓           ↓
             Stop      Docker Build
                           ↓
                        Artifact
                           ↓
                      Future CD
                           ↓
                       Deployment

---

## 27. Step 24 Final Status

# Step 24 — GitHub Actions (CI/CD) → COMPLETE

All planned sub-steps were completed:

    24.1 — GitHub Actions Fundamentals
        ✓

    24.2 — Workflow Setup
        ✓

    24.3 — First CI Pipeline
        ✓

    24.4 — Frontend + Backend CI
        ✓

    24.5 — CI Failure & Debugging
        ✓

    24.6 — Final CI/CD Workflow
        ✓


Step 24 is complete.

