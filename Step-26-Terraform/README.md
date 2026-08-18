```markdown
# Step 26 — Terraform
## Part 1 — Fundamentals

---

## 1. Objective

Step 26 focuses on **Terraform** as an Infrastructure as Code (IaC) tool.

The main objective is to understand how infrastructure can be defined and managed through configuration instead of being created and modified manually.

This step covers the fundamental Terraform workflow:

    Terraform Configuration
            ↓
    terraform init
            ↓
    terraform validate
            ↓
    terraform plan
            ↓
    terraform apply
            ↓
    Infrastructure
            ↓
    Terraform State
            ↓
    Drift Detection / Reconciliation
            ↓
    terraform destroy

The goal is not to memorize Terraform commands, but to understand how Terraform compares desired infrastructure with actual infrastructure and manages the difference.

---

## 2. Environment

### Operating System

    Ubuntu Linux

### Architecture

    linux_amd64

### Terraform

    Terraform v1.15.8

Terraform was verified successfully using:

    terraform version

### Docker

    Docker version 29.7.2
    Docker Engine 29.7.2

Docker was used as the local infrastructure platform for the Terraform practical lab.

### Practice Path

    /home/afroza/Projects/devops-supplementary-labs/Step-26-Terraform/

### Documentation Path

    /home/afroza/Projects/devops-learning/Step-26-Terraform/

### Phase

Step 26 is part of **Phase 2 — Supplementary Labs**.

Terraform practice was kept separate from the Phase 1 MERN project.

The main MERN project was not modified during this step.

---

## 3. Short Theory

### 3.1 Infrastructure as Code

Infrastructure as Code (IaC) means defining infrastructure through code or configuration files instead of creating infrastructure manually through graphical interfaces or individual commands.

Traditional approach:

    Manually create infrastructure
            ↓
    Configure infrastructure
            ↓
    Repeat the process when needed

Terraform approach:

    Write configuration
            ↓
    terraform plan
            ↓
    Review changes
            ↓
    terraform apply
            ↓
    Infrastructure created/updated

This makes infrastructure more reproducible, reviewable, and manageable.

---

### 3.2 Terraform

Terraform is an Infrastructure as Code tool used to define and manage infrastructure through declarative configuration.

Terraform describes the **desired state** of infrastructure.

For example, the practical lab declared:

    docker_image.nginx
    docker_container.nginx

Terraform then compared the configuration with the infrastructure and determined what actions were required.

---

### 3.3 Declarative Configuration

Terraform uses a declarative approach.

Instead of describing every individual action step-by-step, the configuration describes the desired end state.

Example:

    resource "docker_container" "nginx" {
      name  = var.container_name
      image = docker_image.nginx.image_id
    }

The configuration describes what the container should look like.

Terraform determines how to reach that desired state.

### Declarative vs Imperative

| Declarative | Imperative |
|---|---|
| Describes desired result | Describes exact steps |
| Terraform uses this model | Shell scripts commonly use this model |
| Terraform determines required changes | User/program specifies the sequence |
| Focuses on final state | Focuses on actions |

---

### 3.4 Provider

A Terraform provider allows Terraform to communicate with an external platform, service, or API.

The practical lab used:

    kreuzwerker/docker

This provider allows Terraform to communicate with Docker.

Relationship:

    Terraform
        ↓
    Docker Provider
        ↓
    Docker Engine

Terraform itself does not run the Nginx container directly.

The Docker provider translates Terraform's configuration into operations against Docker.

---

### 3.5 Resource

A resource represents an infrastructure object managed by Terraform.

The practical lab used two resources:

    docker_image.nginx

and

    docker_container.nginx

The image represents the Nginx Docker image.

The container represents the running Nginx container.

Relationship:

    docker_image.nginx
            ↓
    docker_container.nginx

The container depends on the image.

---

### 3.6 Terraform State

Terraform maintains state information about the infrastructure it manages.

The local practical lab created:

    terraform.tfstate

The state contains Terraform's recorded information about managed resources.

Basic distinction:

| Configuration | State |
|---|---|
| Describes desired infrastructure | Records managed infrastructure information |
| Written by the user | Maintained by Terraform |
| `.tf` files | `terraform.tfstate` |
| Defines what should exist | Helps Terraform track what exists |

Terraform uses configuration, state, and information obtained from the real infrastructure to determine required changes.

---

### 3.7 Variables

Variables provide configurable input to Terraform configuration.

The practical lab used:

    variable "container_name"

and

    variable "external_port"

Instead of hard-coding values directly inside the resource, the resource used:

    var.container_name

and

    var.external_port

This makes configuration easier to modify and reuse.

---

### 3.8 Outputs

Outputs expose useful information from Terraform-managed resources.

The practical lab defined outputs for:

    container_name

and

    container_port

After applying the output configuration, Terraform displayed:

    container_name = "terraform-nginx"
    container_port = 8081

Variables and outputs serve opposite purposes:

| Variable | Output |
|---|---|
| Input to Terraform | Information returned from Terraform |
| Customizes configuration | Exposes useful values |
| `var.container_name` | `output "container_name"` |

---

## 4. Terraform Architecture / Workflow

The practical Terraform workflow can be represented as:

    Terraform Configuration
            │
            ├── Provider
            ├── Resources
            ├── Variables
            └── Outputs
            │
            ↓
    terraform init
            │
            ↓
    Provider Initialization
            │
            ↓
    terraform validate
            │
            ↓
    Configuration Validation
            │
            ↓
    terraform plan
            │
            ↓
    Compare Desired State
    with Existing Infrastructure
            │
            ↓
    terraform apply
            │
            ↓
    Infrastructure Changes
            │
            ↓
    Terraform State
            │
            ↓
    Verify Actual Infrastructure
            │
            ↓
    Detect Drift When Necessary
            │
            ↓
    Reconcile Infrastructure
            │
            ↓
    terraform destroy
            │
            ↓
    Infrastructure Removed

---

### 4.1 Core Terraform Workflow Commands

| Command | Main purpose |
|---|---|
| `terraform init` | Initializes the Terraform working directory |
| `terraform validate` | Checks whether configuration is valid |
| `terraform plan` | Shows proposed infrastructure changes |
| `terraform apply` | Applies the proposed changes |
| `terraform destroy` | Removes Terraform-managed infrastructure |

The practical lab demonstrated the complete lifecycle rather than using the commands only theoretically.

---

### 4.2 Plan vs Apply

| `terraform plan` | `terraform apply` |
|---|---|
| Preview changes | Perform changes |
| Does not normally modify infrastructure | Modifies infrastructure |
| Helps review proposed actions | Executes the configuration |
| Example: `2 to add` | Example: `2 added` |

Example from the practical lab:

    Plan: 2 to add, 0 to change, 0 to destroy.

After applying:

    Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

---

### 4.3 Desired State vs Actual State

Terraform's fundamental model can be understood as:

    Configuration
          ↓
    Desired State

    Docker Infrastructure
          ↓
    Actual State

Terraform compares these states.

If they match:

    No changes. Your infrastructure matches the configuration.

If they differ:

    Terraform proposes changes through terraform plan.

During the practical lab, changing the desired host port from `8080` to `8081` caused Terraform to detect a difference and propose a container replacement.

---

## 5. Important Differences

### 5.1 Terraform vs Docker

| Terraform | Docker |
|---|---|
| Infrastructure as Code tool | Container platform |
| Defines and manages infrastructure | Builds and runs containers |
| Uses declarative configuration | Provides container runtime |
| Uses providers | Docker Engine directly manages containers |
| `plan`, `apply`, `destroy` | `build`, `run`, `stop`, `rm` |

In this lab they worked together:

    Terraform
        ↓
    Docker Provider
        ↓
    Docker Engine
        ↓
    Nginx Container

---

### 5.2 Terraform vs Ansible

| Terraform | Ansible |
|---|---|
| Mainly infrastructure provisioning/management | Mainly configuration management and automation |
| Declarative infrastructure model | Primarily task-based automation |
| Tracks infrastructure through state | Generally does not use Terraform-style state |
| Commonly used to provision resources | Commonly used to configure existing systems |

They can complement each other, but they solve different primary problems.

---

### 5.3 Terraform vs Kubernetes

| Terraform | Kubernetes |
|---|---|
| Infrastructure provisioning and management | Container orchestration |
| Creates/manages infrastructure resources | Runs and manages containerized workloads |
| Uses providers | Uses Kubernetes API |
| `plan/apply/destroy` | Deployments, Pods, Services, etc. |
| IaC tool | Container orchestration platform |

Conceptually:

    Terraform
        ↓
    Infrastructure

    Kubernetes
        ↓
    Container Workloads

---

### 5.4 Terraform Configuration vs Terraform State

| Configuration | State |
|---|---|
| Desired state | Recorded managed infrastructure |
| Written/edited by user | Maintained by Terraform |
| `.tf` files | `terraform.tfstate` |
| Defines resources | Tracks resources |
| Describes what should exist | Helps Terraform understand what it manages |

The state file should not be treated as a replacement for configuration.

---

### 5.5 Variable vs Output

| Variable | Output |
|---|---|
| Provides input | Provides result/information |
| Used to customize configuration | Used to expose useful resource values |
| `var.external_port` | `container_port` |
| Input direction | Output direction |

---

### 5.6 `plan` vs `apply` vs `destroy`

| Command | Purpose |
|---|---|
| `terraform plan` | Preview what Terraform intends to change |
| `terraform apply` | Create/update infrastructure according to configuration |
| `terraform destroy` | Remove Terraform-managed infrastructure |

---

## Part 1 Summary

The fundamental Terraform model established in this step is:

    Infrastructure as Code
            ↓
    Declarative Configuration
            ↓
    Provider
            ↓
    Resource
            ↓
    Desired State
            ↓
    terraform plan
            ↓
    terraform apply
            ↓
    Infrastructure
            ↓
    Terraform State
            ↓
    Drift Detection / Reconciliation
            ↓
    terraform destroy

Terraform, Docker, and Kubernetes solve different problems:

    Docker
    → Containerization

    Terraform
    → Infrastructure provisioning/management

    Kubernetes
    → Container orchestration

The practical work for these concepts was performed in the Phase 2 supplementary lab and kept separate from the main MERN project.
```



```markdown
# Step 26 — Terraform
## Part 2 — CLI & Configuration

---

## 5. Terraform CLI Commands

The Terraform CLI was used throughout the practical lab to initialize, validate, plan, apply, inspect, modify, and destroy infrastructure.

### 5.1 Check Terraform Version

    terraform version

Purpose:

    Displays the installed Terraform version and platform information.

Actual environment:

    Terraform v1.15.8
    linux_amd64

---

### 5.2 Initialize Terraform

    terraform init

Purpose:

    Initializes the Terraform working directory and installs the required providers.

In this lab, `terraform init` initialized the Docker provider:

    kreuzwerker/docker

Important result:

    Terraform has been successfully initialized!

It also created Terraform provider-related files/directories, including:

    .terraform/
    .terraform.lock.hcl

---

### 5.3 Validate Configuration

    terraform validate

Purpose:

    Checks whether the Terraform configuration is syntactically and structurally valid.

Actual verification:

    Success! The configuration is valid.

`validate` does not create infrastructure.

---

### 5.4 Preview Infrastructure Changes

    terraform plan

Purpose:

    Compares the desired configuration with the current infrastructure/state and shows the changes Terraform proposes.

Example from the practical lab:

    Plan: 2 to add, 0 to change, 0 to destroy.

Later, when the port was changed:

    Plan: 1 to add, 0 to change, 1 to destroy.

The second result demonstrated that Terraform needed to replace the existing container.

---

### 5.5 Apply Configuration

    terraform apply

Purpose:

    Applies the changes proposed by Terraform.

For non-interactive lab execution, the following form was used:

    terraform apply -auto-approve

The practical lab successfully created infrastructure:

    Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

---

### 5.6 Destroy Infrastructure

    terraform destroy

Purpose:

    Removes infrastructure managed by the current Terraform configuration.

The lab used:

    terraform destroy -auto-approve

Final result:

    Destroy complete! Resources: 2 destroyed.

---

### 5.7 Show Terraform State

    terraform show

Purpose:

    Displays the current Terraform state and the resources recorded by Terraform.

This was used to inspect:

    docker_container.nginx
    docker_image.nginx

Important information included:

    container name
    image ID
    network information
    port mapping
    resource ID

---

### 5.8 List Managed Resources

    terraform state list

Purpose:

    Lists resources currently tracked in Terraform state.

During the practical lab, it displayed:

    docker_container.nginx
    docker_image.nginx

After `terraform destroy`, the state list became empty because Terraform no longer managed those resources.

---

### 5.9 Inspect a Specific Resource

    terraform state show docker_container.nginx

Purpose:

    Displays detailed state information for a specific Terraform resource.

The practical lab used filtering to focus on important attributes:

    terraform state show docker_container.nginx | grep -E 'name =|image =|external =|internal ='

This showed information such as:

    name = "terraform-nginx"
    external = 8081
    internal = 80

---

### 5.10 Inspect Providers

    terraform providers

Purpose:

    Displays the providers required by the Terraform configuration.

The lab verified the Docker provider:

    kreuzwerker/docker

---

### 5.11 Show Output Values

    terraform output

Purpose:

    Displays values defined through Terraform `output` blocks.

The lab eventually displayed:

    container_name = "terraform-nginx"
    container_port = 8081

Before the output values had been applied to state, `terraform output` produced:

    Warning: No outputs found

This demonstrated that defining an output in configuration and storing its value in Terraform state are related but separate operations.

---

### 5.12 Format Configuration

    terraform fmt

Purpose:

    Formats Terraform configuration files according to Terraform's standard formatting rules.

This was used after creating/updating Terraform configuration files.

---

### 5.13 Generate Dependency Graph

    terraform graph

Purpose:

    Generates a dependency graph representing relationships between Terraform resources.

The practical lab showed the relationship:

    docker_container.nginx
            ↓
    docker_image.nginx

The container depended on the Docker image because the container configuration referenced the image resource.

---

## 6. Command Explanation

### Core Workflow

    terraform init
    terraform validate
    terraform plan
    terraform apply
    terraform destroy

Meaning:

| Command | What it does |
|---|---|
| `init` | Prepares the Terraform working directory |
| `validate` | Checks configuration validity |
| `plan` | Previews changes |
| `apply` | Performs changes |
| `destroy` | Removes managed infrastructure |

---

### State Commands

    terraform state list

Lists resources managed by Terraform.

    terraform state show <resource>

Shows detailed state information for one resource.

    terraform show

Shows the current Terraform state in a broader form.

---

### Inspection Commands

    terraform providers

Shows required providers.

    terraform output

Shows configured output values.

    terraform graph

Shows Terraform resource relationships.

---

## 7. Important Flags

### `-auto-approve`

Used with:

    terraform apply -auto-approve

and:

    terraform destroy -auto-approve

Purpose:

    Skips the interactive approval prompt.

This was appropriate for the controlled local learning lab.

In production workflows, automatic approval should be used carefully because it removes the manual confirmation step.

---

### `-E` with grep

The practical resource inspection used:

    grep -E 'name =|image =|external =|internal ='

`-E` enables extended regular expressions.

It allowed several fields to be selected in one command.

---

### `--filter` with Docker

The practical verification used:

    docker ps --filter "name=terraform-nginx"

Purpose:

    Shows only Docker containers matching the specified filter.

This made verification easier by focusing on the Terraform-managed container.

---

### `--max-time` with curl

During the old-port verification, the command used:

    curl -I --max-time 3 http://localhost:8080

`--max-time 3` limits the maximum time curl waits for the request.

This prevented the verification command from waiting unnecessarily when port `8080` was no longer serving the container.

---

## 8. Command Variations

### Apply

Interactive:

    terraform apply

Non-interactive:

    terraform apply -auto-approve

The interactive form is useful when manually reviewing and approving changes.

The `-auto-approve` form is useful for controlled automation or lab execution.

---

### Destroy

Interactive:

    terraform destroy

Non-interactive:

    terraform destroy -auto-approve

Both perform the same fundamental operation; the difference is whether Terraform waits for interactive approval.

---

### Resource Inspection

General state:

    terraform show

Specific resource:

    terraform state show docker_container.nginx

Listing resources:

    terraform state list

These commands answer different questions:

| Command | Main question |
|---|---|
| `terraform show` | What does the current state contain? |
| `terraform state list` | Which resources are managed? |
| `terraform state show <resource>` | What details are recorded for this resource? |

---

### Docker Verification

General running containers:

    docker ps

Filtered verification:

    docker ps --filter "name=terraform-nginx"

All containers, including stopped ones:

    docker ps -a

The practical lab used these variations to verify creation, running status, manual stopping, and cleanup.

---

## 9. Configuration

The Terraform lab used three main configuration files:

    main.tf
    variables.tf
    outputs.tf

Terraform also generated:

    .terraform/
    .terraform.lock.hcl
    terraform.tfstate

---

### 9.1 `main.tf`

The main configuration contained the Terraform configuration, Docker provider, image resource, and container resource.

Actual configuration:

    terraform {
      required_providers {
        docker = {
          source  = "kreuzwerker/docker"
          version = "~> 3.0"
        }
      }
    }

    provider "docker" {}

    resource "docker_image" "nginx" {
      name = "nginx:alpine"
    }

    resource "docker_container" "nginx" {
      name  = var.container_name
      image = docker_image.nginx.image_id

      ports {
        internal = 80
        external = var.external_port
      }
    }

---

### 9.2 Required Provider

The configuration declared:

    source = "kreuzwerker/docker"

and:

    version = "~> 3.0"

The provider allowed Terraform to communicate with Docker.

The provider was initialized with:

    terraform init

The installed provider version was resolved by Terraform and recorded in:

    .terraform.lock.hcl

---

### 9.3 Docker Image Resource

The practical lab defined:

    resource "docker_image" "nginx" {
      name = "nginx:alpine"
    }

This instructed Terraform to manage the Nginx Alpine Docker image.

---

### 9.4 Docker Container Resource

The practical lab defined:

    resource "docker_container" "nginx" {
      name  = var.container_name
      image = docker_image.nginx.image_id

      ports {
        internal = 80
        external = var.external_port
      }
    }

Important configuration:

    name = var.container_name

Uses a variable for the container name.

    image = docker_image.nginx.image_id

References the image resource.

This creates an implicit dependency:

    docker_image.nginx
            ↓
    docker_container.nginx

Port configuration:

    internal = 80
    external = var.external_port

The Nginx container listened on port `80`, while the Ubuntu host exposed it through the configured external port.

---

### 9.5 `variables.tf`

The lab defined:

    variable "container_name" {
      description = "Name of the Nginx container"
      type        = string
      default     = "terraform-nginx"
    }

    variable "external_port" {
      description = "Host port for Nginx"
      type        = number
      default     = 8081
    }

Variables separate configurable values from the resource definition.

---

### 9.6 `outputs.tf`

The lab defined:

    output "container_name" {
      description = "Name of the managed Nginx container"
      value       = docker_container.nginx.name
    }

    output "container_port" {
      description = "Host port exposed for Nginx"
      value       = docker_container.nginx.ports[0].external
    }

These outputs exposed useful information from the managed Docker container.

The resulting values were:

    container_name = "terraform-nginx"
    container_port = 8081

---

### 9.7 Terraform State

The practical lab generated:

    terraform.tfstate

The state recorded information about resources managed by Terraform.

Examples included:

    docker_container.nginx
    docker_image.nginx

The state also contained information such as:

    resource IDs
    image IDs
    container name
    port mappings
    network information

The state was used during subsequent `terraform plan` operations to compare the desired configuration with the infrastructure Terraform was managing.

---

### 9.8 Configuration Change Demonstration

The practical lab initially used:

    external_port = 8080

Later it was changed to:

    external_port = 8081

Terraform detected the difference during:

    terraform plan

The plan indicated that the Docker container required replacement:

    external = 8080 -> 8081 # forces replacement

Terraform then performed:

    destroy old container
    create new container

The new container exposed:

    0.0.0.0:8081->80/tcp

The application was verified using:

    curl -I http://localhost:8081

The response returned:

    HTTP/1.1 200 OK

---

### 9.9 Configuration and State Relationship

The practical configuration can be summarized as:

    main.tf
        ↓
    Defines resources and desired configuration

    variables.tf
        ↓
    Defines configurable inputs

    outputs.tf
        ↓
    Defines useful returned values

    terraform.tfstate
        ↓
    Records Terraform-managed infrastructure information

Together, these components allowed Terraform to understand, manage, inspect, modify, and eventually destroy the infrastructure used in the practical lab.

---

## Part 2 Summary

The Terraform CLI and configuration workflow used in this step was:

    main.tf
    variables.tf
    outputs.tf
            ↓
    terraform init
            ↓
    terraform validate
            ↓
    terraform plan
            ↓
    terraform apply
            ↓
    terraform state / show / output
            ↓
    configuration change
            ↓
    terraform plan
            ↓
    infrastructure replacement
            ↓
    drift detection and reconciliation
            ↓
    terraform destroy

The configuration remained in the supplementary lab after destruction, while the Docker infrastructure managed by Terraform was removed.
```


```markdown
# Step 26 — Terraform
## Part 3 — Practical Lab & Troubleshooting

---

## 10. Practical Work

The Terraform practical lab was performed separately from the main MERN project.

Practice directory:

    /home/afroza/Projects/devops-supplementary-labs/Step-26-Terraform/

The lab used Docker as a local infrastructure platform to avoid unnecessary cloud credentials and billing.

The practical infrastructure consisted of:

    Terraform
        ↓
    Docker Provider
        ↓
    Docker Engine
        ↓
    nginx:alpine
        ↓
    terraform-nginx container

---

### 10.1 Initial Terraform Configuration

The first Terraform configuration declared the Docker provider.

The provider was initialized with:

    terraform init

The configuration was then validated with:

    terraform validate

Result:

    Success! The configuration is valid.

No infrastructure was created during initialization or validation.

---

### 10.2 First Terraform Resources

Two Terraform resources were created:

    docker_image.nginx

and:

    docker_container.nginx

The image resource managed:

    nginx:alpine

The container resource managed:

    terraform-nginx

The container exposed:

    Host: 8080
    Container: 80

The initial plan showed:

    Plan: 2 to add, 0 to change, 0 to destroy.

This demonstrated that Terraform had identified two resources that did not yet exist.

---

### 10.3 Applying the Initial Infrastructure

The configuration was applied with:

    terraform apply -auto-approve

Terraform created the image first:

    docker_image.nginx: Creation complete

Then it created the container:

    docker_container.nginx: Creation complete

Final result:

    Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

This demonstrated the dependency between the Docker image and container.

---

### 10.4 Application Verification

The Docker container was verified with:

    docker ps --filter "name=terraform-nginx"

The container was running and exposed:

    0.0.0.0:8080->80/tcp

The Nginx application was then tested with:

    curl http://localhost:8080

The response contained:

    Welcome to nginx!

This verified the infrastructure at both the container level and application level.

---

### 10.5 Variables

The configuration was later changed to use Terraform variables.

The variables included:

    container_name

and:

    external_port

The container resource used:

    name = var.container_name

and:

    external = var.external_port

The default external port was later changed from:

    8080

to:

    8081

This demonstrated how variables separate configurable values from resource definitions.

---

### 10.6 Outputs

Terraform outputs were added for:

    container_name

and:

    container_port

The first `terraform plan` after adding the outputs showed:

    Changes to Outputs:
      + container_name = "terraform-nginx"
      + container_port = 8080

At that point, the output values had not yet been stored in state.

Running:

    terraform output

produced:

    Warning: No outputs found

After applying the output configuration, Terraform displayed:

    container_name = "terraform-nginx"
    container_port = 8080

This demonstrated that output definitions become available through `terraform output` after the values are applied to Terraform state.

---

## 11. What to Observe

The practical lab used observation as part of every major Terraform operation.

### 11.1 After `terraform init`

Observe:

    Terraform has been successfully initialized!

Also observe the creation of provider-related files/directories such as:

    .terraform/
    .terraform.lock.hcl

Meaning:

The working directory has been initialized and the required Docker provider has been installed/locked.

---

### 11.2 After `terraform validate`

Expected result:

    Success! The configuration is valid.

Meaning:

Terraform can parse and validate the configuration successfully.

---

### 11.3 After `terraform plan`

The first plan showed:

    Plan: 2 to add, 0 to change, 0 to destroy.

Meaning:

Terraform identified two resources that needed to be created.

---

### 11.4 After `terraform apply`

Expected result:

    Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Meaning:

The planned infrastructure was actually created.

---

### 11.5 After Docker Verification

The container showed:

    Up

and:

    0.0.0.0:8080->80/tcp

Meaning:

The container was running and the host-to-container port mapping was active.

---

### 11.6 After `curl`

The Nginx response contained:

    Welcome to nginx!

Meaning:

The container was not only running, but the web server was reachable through the exposed port.

---

### 11.7 After Changing the Port

The desired port was changed:

    8080 → 8081

The plan showed:

    external = 8080 -> 8081 # forces replacement

Meaning:

The Docker container could not perform this particular change in place through the provider, so Terraform planned to replace the container.

---

### 11.8 After Applying the Port Change

Terraform reported:

    Apply complete! Resources: 1 added, 0 changed, 1 destroyed.

Docker then showed:

    0.0.0.0:8081->80/tcp

Nginx was verified through:

    curl -I http://localhost:8081

The response returned:

    HTTP/1.1 200 OK

The old port `8080` was no longer serving the Nginx container.

---

### 11.9 After the Final Plan

After the port replacement, the final plan returned:

    No changes. Your infrastructure matches the configuration.

Meaning:

    Desired State = Actual Infrastructure

Terraform did not need to make any further changes.

---

## 12. Actual Output Meaning

### 12.1 `2 to add`

Example:

    Plan: 2 to add, 0 to change, 0 to destroy.

Meaning:

Two resources are absent from the current infrastructure and need to be created.

In this lab:

    docker_image.nginx
    docker_container.nginx

---

### 12.2 `1 to add, 1 to destroy`

When the port changed from `8080` to `8081`, Terraform showed:

    Plan: 1 to add, 0 to change, 1 to destroy.

Meaning:

The existing container needed to be replaced.

The image did not need to be recreated because its configuration had not changed.

---

### 12.3 `No changes`

The message:

    No changes. Your infrastructure matches the configuration.

means Terraform compared the current infrastructure with the configuration and found no difference requiring action.

This is an important Terraform verification result.

---

### 12.4 Terraform State

The lab inspected state with:

    terraform state list

The resources were:

    docker_container.nginx
    docker_image.nginx

The container state was inspected with:

    terraform state show docker_container.nginx

Important recorded information included:

    name = "terraform-nginx"
    external = 8081
    internal = 80

The state therefore contained information about the infrastructure Terraform was managing.

---

### 12.5 Dependency

The dependency graph showed a relationship between:

    docker_container.nginx

and:

    docker_image.nginx

The container configuration referenced:

    docker_image.nginx.image_id

Therefore the container depended on the image.

Conceptually:

    docker_image.nginx
            ↓
    docker_container.nginx

Terraform could therefore determine the appropriate resource relationship automatically.

---

## 13. Differences

### 13.1 Desired State vs Actual State

| Desired State | Actual State |
|---|---|
| Defined by Terraform configuration | Exists in Docker |
| Describes what should exist | Represents what actually exists |
| Example: port `8081` | Example: container exposing `8081` |
| Source: `.tf` configuration | Source: real infrastructure/provider information |

Terraform compares these states to determine required actions.

---

### 13.2 Configuration vs State

| Configuration | State |
|---|---|
| Defines desired infrastructure | Records managed infrastructure information |
| `.tf` files | `terraform.tfstate` |
| Written/edited by the user | Maintained by Terraform |
| Remains as configuration after destroy | Managed resource entries are removed after destroy |

---

### 13.3 Resource vs Provider

| Provider | Resource |
|---|---|
| Connects Terraform to an external platform | Represents an infrastructure object |
| Example: Docker provider | Example: Docker container |
| Enables Terraform to communicate with Docker | Defines what Terraform manages |

---

## 14. Troubleshooting

### 14.1 `terraform output` — No Outputs Found

Observed situation:

    Warning: No outputs found

Cause:

Output blocks had been added to configuration, but their values had not yet been stored in Terraform state.

Resolution:

    terraform apply -auto-approve

Verification:

    terraform output

Result:

    container_name = "terraform-nginx"
    container_port = 8080

---

### 14.2 Configuration Change Requires Replacement

Observed plan:

    external = 8080 -> 8081 # forces replacement

Cause:

The Docker container resource could not apply this particular port change in place.

Terraform therefore planned:

    destroy old container
            ↓
    create new container

Verification after apply:

    0.0.0.0:8081->80/tcp

and:

    HTTP/1.1 200 OK

---

### 14.3 Drift Detection

A manual change was deliberately introduced outside Terraform:

    docker stop terraform-nginx

Docker then showed:

    Exited (0)

The desired Terraform configuration still required the container to exist and run.

Running:

    terraform plan

produced:

    Plan: 1 to add, 0 to change, 0 to destroy.

Terraform recognized that the required container was no longer present in the expected state and proposed creating it again.

---

### 14.4 Drift Reconciliation

The drift was corrected with:

    terraform apply -auto-approve

Terraform created a new container.

Result:

    Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Docker verification showed:

    Up

and:

    0.0.0.0:8081->80/tcp

A final plan returned:

    No changes. Your infrastructure matches the configuration.

This demonstrated Terraform's reconciliation behavior.

---

### 14.5 Important Troubleshooting Workflow

A practical Terraform troubleshooting flow is:

    Check configuration
          ↓
    terraform validate
          ↓
    Inspect current state
          ↓
    terraform state list
          ↓
    Refresh/compare through
    terraform plan
          ↓
    Understand proposed action
          ↓
    terraform apply
          ↓
    Verify actual infrastructure
          ↓
    terraform plan
          ↓
    Confirm "No changes"

The practical lab followed this approach when testing configuration changes and infrastructure drift.

---

## Practical Lab Result

The Terraform lab successfully demonstrated:

    Provider initialization
    Resource creation
    Variables
    Outputs
    Terraform state
    Resource dependencies
    Plan and apply workflow
    Configuration changes
    Resource replacement
    Drift detection
    Drift reconciliation
    Infrastructure verification
    Infrastructure destruction

The practical environment remained local and used Docker, avoiding cloud billing and external infrastructure costs.
```
```markdown
# Step 26 — Terraform
## Part 4 — Final Verification & Lessons

---

## 15. Project Integration

Step 26 was implemented as a **Phase 2 supplementary lab**.

The Terraform practical work was intentionally kept separate from the Phase 1 MERN project.

### Practice Location

    /home/afroza/Projects/devops-supplementary-labs/Step-26-Terraform/

### Main MERN Project

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

The MERN project was not modified for the Terraform practical.

### Documentation Location

    /home/afroza/Projects/devops-learning/Step-26-Terraform/

### Why Terraform Was Practiced Separately

Terraform is primarily concerned with:

    Infrastructure provisioning
    Infrastructure management
    Desired infrastructure state

The existing MERN project had already been used for the relevant Phase 1 Docker, Docker Compose, GitHub Actions, and Cloud Fundamentals work.

Terraform was therefore practiced independently to understand its infrastructure-management role without unnecessarily forcing it into the application project.

### Relationship with Previous DevOps Steps

    Docker
        ↓
    Containerization

    Docker Compose
        ↓
    Multi-container local environment

    Terraform
        ↓
    Infrastructure provisioning and management

    Kubernetes
        ↓
    Container orchestration

These technologies solve different problems and can be combined in larger production environments, but they do not need to be artificially combined in every learning project.

---

## 16. Security Considerations

Although the practical lab used only local Docker infrastructure, several Terraform security principles are important.

### 16.1 Protect Terraform State

Terraform state can contain sensitive infrastructure information and, depending on the configuration, may contain sensitive values.

The practical lab generated:

    terraform.tfstate

Therefore state files should not be treated as ordinary harmless configuration files.

Important practices:

    Protect terraform.tfstate
    Do not expose state publicly
    Avoid committing sensitive state to public repositories
    Use appropriate remote state and access controls in production

---

### 16.2 Do Not Hard-Code Credentials

Cloud credentials, API tokens, passwords, and other secrets should not be placed directly inside Terraform configuration files.

Avoid patterns such as:

    password = "real-password"
    access_key = "real-secret"

Production Terraform environments should use secure credential mechanisms and secret-management systems.

---

### 16.3 Principle of Least Privilege

Terraform credentials should have only the permissions required for the infrastructure operations they perform.

Avoid giving Terraform unnecessarily broad administrative permissions.

A production setup should follow:

    Minimum required permissions
            ↓
    Limited blast radius
            ↓
    Better security

---

### 16.4 Review `terraform plan`

Before applying infrastructure changes, review the plan carefully.

The practical workflow demonstrated:

    terraform plan
            ↓
    Review proposed changes
            ↓
    terraform apply

This is particularly important when infrastructure changes could affect production systems or incur cloud costs.

---

### 16.5 Be Careful with `-auto-approve`

The lab used:

    terraform apply -auto-approve

and:

    terraform destroy -auto-approve

because the environment was a controlled local learning lab.

`-auto-approve` removes the interactive approval step.

For production infrastructure, automatic approval should only be used when the surrounding CI/CD workflow provides appropriate review and safety controls.

---

### 16.6 Avoid Unnecessary Cloud Costs

The Terraform lab used local Docker infrastructure instead of a cloud provider.

This avoided:

    Cloud credentials
    Cloud resource charges
    Accidental infrastructure costs

This was appropriate for the learning objective.

---

## 17. Verification

The Terraform practical was verified at multiple levels.

### 17.1 Terraform Installation

Verified:

    Terraform v1.15.8
    linux_amd64

---

### 17.2 Docker Environment

Verified:

    Docker version 29.7.2
    Docker Engine 29.7.2

---

### 17.3 Provider

The Docker provider was initialized successfully:

    kreuzwerker/docker

---

### 17.4 Configuration

Verified with:

    terraform validate

Result:

    Success! The configuration is valid.

---

### 17.5 Initial Infrastructure

Terraform successfully created:

    docker_image.nginx
    docker_container.nginx

Result:

    Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

---

### 17.6 Application

The Nginx container was verified through Docker and HTTP.

Docker showed:

    0.0.0.0:8080->80/tcp

Later, after the configuration change:

    0.0.0.0:8081->80/tcp

HTTP verification returned:

    HTTP/1.1 200 OK

and the Nginx welcome page was returned.

---

### 17.7 Configuration Change

The external port was changed:

    8080 → 8081

Terraform detected the change and planned a replacement.

The replacement was successfully applied.

Final infrastructure matched the new desired state.

---

### 17.8 Drift Detection

A manual infrastructure change was introduced:

    docker stop terraform-nginx

Terraform detected the difference through:

    terraform plan

It then restored the desired infrastructure through:

    terraform apply

Final verification returned:

    No changes. Your infrastructure matches the configuration.

---

### 17.9 Final Cleanup

The lab infrastructure was removed with:

    terraform destroy -auto-approve

Result:

    Destroy complete! Resources: 2 destroyed.

Docker verification showed no remaining `terraform-nginx` container.

Terraform state no longer contained managed resources.

---

### 17.10 Final State After Cleanup

The configuration files remain in the Terraform practice directory.

The infrastructure created during the lab was destroyed.

Therefore:

    Terraform Configuration
        ↓
    Still exists

    Docker Lab Infrastructure
        ↓
    Destroyed

The final `terraform plan` correctly proposed creating the two resources again because the configuration still defines them while the actual infrastructure no longer exists.

Result:

    Plan: 2 to add, 0 to change, 0 to destroy.

This is expected after a successful `terraform destroy`.

---

## 18. Key Lessons

### 18.1 Infrastructure as Code

Infrastructure can be described using configuration instead of being created manually.

---

### 18.2 Terraform Is Declarative

Terraform describes the desired end state.

Terraform determines the required actions to move infrastructure toward that state.

---

### 18.3 Provider

A provider connects Terraform to an external platform.

In this lab:

    Terraform
        ↓
    Docker Provider
        ↓
    Docker Engine

---

### 18.4 Resource

Resources represent infrastructure objects managed by Terraform.

The practical resources were:

    docker_image.nginx
    docker_container.nginx

---

### 18.5 Plan Before Apply

`terraform plan` provides a preview of infrastructure changes.

`terraform apply` performs those changes.

This separation allows changes to be reviewed before execution.

---

### 18.6 State Is Important

Terraform state records information about managed infrastructure.

It allows Terraform to reason about resources and determine what changes are required.

---

### 18.7 Dependencies Matter

The container depended on the image:

    docker_image.nginx
            ↓
    docker_container.nginx

Terraform detected this relationship through the resource reference.

---

### 18.8 Variables Improve Configuration

Variables allow configurable values to be separated from resource definitions.

Example:

    var.external_port

This made changing the external port from `8080` to `8081` straightforward.

---

### 18.9 Outputs Expose Useful Information

Outputs allowed useful resource information to be displayed:

    container_name = "terraform-nginx"
    container_port = 8081

---

### 18.10 Terraform Detects Drift

A manual change outside Terraform caused infrastructure to differ from the desired configuration.

Terraform detected the difference and proposed corrective action.

This demonstrated the reconciliation model:

    Desired State
          ↓
    Compare with Actual State
          ↓
    Difference detected
          ↓
    Apply correction
          ↓
    Desired State restored

---

### 18.11 `No Changes` Is an Important Result

The message:

    No changes. Your infrastructure matches the configuration.

means Terraform found no required infrastructure changes.

This is a useful verification result, not an indication that Terraform did nothing useful.

---

### 18.12 Destroy Does Not Delete Configuration

`terraform destroy` removes managed infrastructure.

It does not remove the Terraform configuration files.

Therefore after destroy:

    Configuration exists
    Infrastructure does not exist

Running `terraform plan` then correctly proposes recreating the defined resources.

---

## 19. Final Result

### Step 26 — Terraform Status

    COMPLETE

### Practical Environment

    /home/afroza/Projects/devops-supplementary-labs/Step-26-Terraform/

### Documentation

    /home/afroza/Projects/devops-learning/Step-26-Terraform/

### Terraform Version

    Terraform v1.15.8

### Docker Version

    Docker 29.7.2
    Docker Engine 29.7.2

### Concepts Practically Covered

    Infrastructure as Code
    Declarative configuration
    Terraform CLI
    Provider
    Resource
    Variables
    Outputs
    Configuration files
    terraform init
    terraform validate
    terraform plan
    terraform apply
    terraform destroy
    Terraform state
    Dependencies
    Desired state
    Actual state
    Configuration drift
    Drift detection
    Reconciliation
    Resource replacement
    Verification
    Troubleshooting

### Practical Infrastructure

The lab successfully created and managed:

    nginx:alpine
    terraform-nginx

The container was successfully exposed through:

    8080 → 80

and later:

    8081 → 80

Nginx was verified through HTTP requests.

The infrastructure was intentionally destroyed at the end of the lab.

### Final Learning Outcome

The complete Terraform lifecycle was practically demonstrated:

    Configuration
          ↓
    Initialize
          ↓
    Validate
          ↓
    Plan
          ↓
    Apply
          ↓
    Verify
          ↓
    Modify
          ↓
    Detect Drift
          ↓
    Reconcile
          ↓
    Destroy

Step 26 established the foundational understanding required before moving to the next orchestration-focused technology:

    Step 26 — Terraform
            ↓
    Infrastructure Provisioning
            ↓
    Step 27 — Kubernetes
            ↓
    Container Orchestration

---

## Final Status

**Step 26 — Terraform: COMPLETE**

Terraform was learned and practiced as a separate Phase 2 supplementary lab without modifying the main MERN project.
```




