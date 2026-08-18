# Step 26 — Terraform

## Part 1 — Fundamentals & Core Concepts

---

## 1. Objective

Terraform is an Infrastructure as Code (IaC) tool used to define, provision, and manage infrastructure through configuration files.

The main objective of this step is to understand and practically use the Terraform workflow:

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
    Verify
            ↓
    terraform destroy

This step focuses on the Terraform fundamentals required for infrastructure management without introducing unnecessary advanced Terraform topics.

---

## 2. Environment

| Component | Environment |
|---|---|
| OS | Ubuntu Linux |
| Architecture | linux_amd64 |
| Terraform | v1.15.8 |
| Docker | 29.7.2 |
| Docker Engine | 29.7.2 |
| Terraform Provider | kreuzwerker/docker |
| Practice Type | Phase 2 Supplementary Lab |
| Practice Path | `/home/afroza/Projects/devops-supplementary-labs/Step-26-Terraform/` |
| Documentation Path | `/home/afroza/Projects/devops-learning/Step-26-Terraform/` |

The Terraform lab was performed separately from the main MERN project.

Main MERN project:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

The main MERN project was not modified for the Terraform lab.

---

## 3. Short Theory

### 3.1 Infrastructure as Code

Infrastructure as Code means defining infrastructure using configuration instead of creating and managing everything manually.

Without IaC:

    Manual Configuration
            ↓
    Manual Changes
            ↓
    Difficult to Reproduce

With IaC:

    Configuration File
            ↓
    Terraform
            ↓
    Infrastructure

The configuration can be reviewed, reused, and version controlled.

---

### 3.2 Declarative Configuration

Terraform uses a declarative approach.

Instead of describing every individual command required to create infrastructure, we describe the desired state.

For example:

    resource "docker_container" "nginx" {
        name = "terraform-nginx"
    }

This describes what should exist.

Terraform determines the actions required to reach that desired state.

| Declarative | Imperative |
|---|---|
| Describe desired state | Describe exact steps |
| Terraform | Shell scripts are a common example |
| Terraform determines required actions | User defines the sequence of actions |
| Focuses on what should exist | Focuses on how to create it |

---

### 3.3 Terraform Architecture

The basic Terraform architecture used in this lab was:

    Terraform CLI
          ↓
    Terraform Configuration
          ↓
    Docker Provider
          ↓
    Docker Engine
          ↓
    Docker Infrastructure

The Docker provider allows Terraform to communicate with Docker.

---

## 4. Terraform Core Components

### 4.1 Terraform Configuration

Terraform configuration is written in `.tf` files.

Example:

    main.tf

The configuration describes the desired infrastructure.

---

### 4.2 Provider

A provider allows Terraform to communicate with a specific platform or service.

In this lab:

    Terraform
        ↓
    Docker Provider
        ↓
    Docker

The Docker provider used was:

    kreuzwerker/docker

---

### 4.3 Resource

A resource represents an infrastructure object managed by Terraform.

The practical lab used:

    docker_image.nginx

and:

    docker_container.nginx

The relationship was:

    docker_image.nginx
            ↓
    docker_container.nginx

The container uses the Docker image, so Terraform can determine the dependency between them.

---

### 4.4 Variables

Variables allow configurable values to be separated from the main resource configuration.

For example:

    var.external_port

Instead of hard-coding a port directly into the resource, the value can be provided through a variable.

This makes configuration easier to change and reuse.

---

### 4.5 Outputs

Outputs expose useful information from Terraform configuration and state.

The practical lab created outputs for:

    container_name

and:

    container_port

Example output:

    container_name = "terraform-nginx"
    container_port = 8081

---

### 4.6 State

Terraform maintains state information about managed infrastructure.

The local lab used:

    terraform.tfstate

The state allows Terraform to compare the configuration with the infrastructure it manages.

Conceptually:

    Configuration
          ↓
    Desired State

    Terraform State
          ↓
    Known Managed Infrastructure

    Actual Infrastructure
          ↓
    Real Resources

Terraform uses these pieces of information to determine what changes are required.

---

## 5. Terraform vs Docker

| Feature | Terraform | Docker |
|---|---|---|
| Main purpose | Infrastructure management | Containerization |
| Defines | Infrastructure/resources | Containers/images |
| Configuration | Terraform `.tf` files | Dockerfile / CLI / Compose |
| State management | Terraform state | Docker's own runtime state |
| Example | Create/manage Docker container | Run container |
| Main focus | Desired infrastructure state | Running containers |

In this lab, Terraform was used to manage Docker infrastructure.

---

## 6. Terraform vs Kubernetes

| Feature | Terraform | Kubernetes |
|---|---|---|
| Main purpose | Infrastructure provisioning/management | Container orchestration |
| Main focus | Infrastructure | Running workloads |
| Typical object | Resource | Pod, Deployment, Service |
| Desired state | Infrastructure state | Application/cluster state |
| Example | Create infrastructure | Run and scale containers |

Terraform and Kubernetes solve different problems and can be used together in larger DevOps environments.

---

## 7. Terraform vs Ansible

| Feature | Terraform | Ansible |
|---|---|---|
| Main focus | Infrastructure provisioning | Configuration/automation |
| Approach | Declarative | Primarily task-based |
| Common use | Create infrastructure | Configure existing systems |
| State | Maintains Terraform state | Does not use Terraform-style state |
| Example | Create a server/network/resource | Install packages and configure services |

---

## 8. Core Terraform Workflow

The practical Terraform lifecycle used in this step was:

    Write Configuration
            ↓
    terraform init
            ↓
    terraform validate
            ↓
    terraform plan
            ↓
    terraform apply
            ↓
    Verify Infrastructure
            ↓
    Modify Configuration
            ↓
    terraform plan
            ↓
    terraform apply
            ↓
    terraform destroy

Each stage has a different purpose.

| Command | Purpose |
|---|---|
| `terraform init` | Initialize the Terraform working directory |
| `terraform validate` | Check whether configuration is valid |
| `terraform plan` | Preview infrastructure changes |
| `terraform apply` | Apply the desired infrastructure |
| `terraform output` | Display defined output values |
| `terraform state` | Inspect/manage Terraform state |
| `terraform destroy` | Remove managed infrastructure |

---

## 9. Important Concept

Terraform does not simply execute commands one after another.

Its main idea is:

    Desired Configuration
            ↓
        Compare
            ↓
    Current Infrastructure
            ↓
    Calculate Difference
            ↓
    Proposed Changes
            ↓
        Apply Changes

This difference-based workflow is the foundation of Terraform.

The practical lab later demonstrated this by:

    Creating the Nginx container
            ↓
    Changing the external port
            ↓
    Terraform detecting the difference
            ↓
    Replacing the container
            ↓
    Detecting manual drift
            ↓
    Restoring the desired state


# Step 26 — Terraform

## Part 2 — Practical Configuration & Workflow

---

## 10. Practice Environment

Terraform practice was performed in the Phase 2 supplementary lab:

    /home/afroza/Projects/devops-supplementary-labs/Step-26-Terraform

The lab used Docker as the local infrastructure platform.

The practical setup was:

    Terraform
        ↓
    Docker Provider
        ↓
    Docker Engine
        ↓
    nginx:alpine
        ↓
    terraform-nginx

This allowed Terraform concepts to be practiced locally without requiring cloud credentials or cloud billing.

---

## 11. Terraform Configuration Files

The main Terraform configuration file was:

    main.tf

The configuration declared the Docker provider and the Docker resources required for the lab.

Terraform also created its local working/state-related files during the workflow.

Important files/directories observed:

    main.tf
    .terraform/
    .terraform.lock.hcl
    terraform.tfstate

| File / Directory | Purpose |
|---|---|
| `main.tf` | Defines the desired infrastructure |
| `.terraform/` | Terraform working directory containing initialized provider/plugin data |
| `.terraform.lock.hcl` | Records the selected provider version information |
| `terraform.tfstate` | Records Terraform-managed infrastructure state |

---

## 12. Terraform Initialization

The first major Terraform command was:

    terraform init

Purpose:

    Initialize the Terraform working directory
    ↓
    Install required providers
    ↓
    Prepare Terraform for other commands

The Docker provider was installed successfully.

Observed provider:

    kreuzwerker/docker v3.9.0

Terraform reported:

    Terraform has been successfully initialized!

The initialization also created:

    .terraform/
    .terraform.lock.hcl

---

## 13. Terraform Validation

After initialization, the configuration was checked with:

    terraform validate

Result:

    Success! The configuration is valid.

This verified that Terraform could successfully parse and validate the configuration.

`terraform validate` does not create infrastructure.

---

## 14. Terraform Formatting

The configuration was formatted using:

    terraform fmt

Purpose:

    Format Terraform configuration using Terraform's standard style.

Formatting improves consistency and readability.

---

## 15. Creating the Docker Image Resource

The first infrastructure resource was:

    docker_image.nginx

The image used was:

    nginx:alpine

Terraform was responsible for ensuring that the required Docker image existed.

Conceptually:

    Terraform Configuration
            ↓
    docker_image.nginx
            ↓
    nginx:alpine

---

## 16. Creating the Docker Container Resource

The second resource was:

    docker_container.nginx

The container name was:

    terraform-nginx

The container used the Nginx image and exposed:

    Host Port: 8080
    Container Port: 80

The port mapping was:

    0.0.0.0:8080 → 80/tcp

The important relationship was:

    docker_image.nginx
            ↓
    docker_container.nginx

Terraform determined that the container depended on the image.

---

## 17. Terraform Plan

After configuring the resources, the infrastructure was previewed using:

    terraform plan

The initial plan showed:

    Plan: 2 to add, 0 to change, 0 to destroy.

The two resources were:

    docker_image.nginx
    docker_container.nginx

The `+` symbol in the plan represented a resource that Terraform intended to create.

Important idea:

    terraform plan
        =
    Preview only

It does not create or modify the infrastructure.

---

## 18. Terraform Apply

The planned infrastructure was created using:

    terraform apply -auto-approve

Terraform created:

    docker_image.nginx

followed by:

    docker_container.nginx

The final result was:

    Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

The Docker container was then running with:

    0.0.0.0:8080->80/tcp

---

## 19. Verify the Docker Container

The container was verified using:

    docker ps --filter "name=terraform-nginx"

Important output:

    STATUS
    Up

and:

    PORTS
    0.0.0.0:8080->80/tcp

This proved that the Terraform-managed container was running and the port mapping was active.

---

## 20. Verify Nginx Through HTTP

The Nginx service was tested using:

    curl http://localhost:8080

The response contained:

    Welcome to nginx!

This verification was important because it checked the complete path:

    Terraform
        ↓
    Docker Container
        ↓
    Nginx
        ↓
    Host Port 8080
        ↓
    HTTP Request

Therefore the infrastructure was not only created but also functionally reachable.

---

## 21. Variables

Variables were introduced to make the configuration more flexible.

The external port was represented using:

    var.external_port

The variable was defined with:

    variable "external_port" {
        description = "Host port for Nginx"
        type        = number
        default     = 8081
    }

The resource then used the variable instead of a fixed port value.

| Without Variable | With Variable |
|---|---|
| `external = 8080` | `external = var.external_port` |
| Value directly inside resource | Value controlled by variable |
| Less flexible | More reusable |

---

## 22. Outputs

Outputs were added for useful Terraform information.

The configured outputs were:

    container_name

and:

    container_port

After applying the output configuration, Terraform displayed:

    container_name = "terraform-nginx"
    container_port = 8080

Later, after changing the port:

    container_name = "terraform-nginx"
    container_port = 8081

Outputs provide a convenient way to expose important values from Terraform state.

---

## 23. Configuration Change

The external Docker port was changed:

    8080 → 8081

Terraform was then asked to calculate the difference using:

    terraform plan

The plan showed:

    external = 8080 -> 8081 # forces replacement

Terraform therefore planned:

    Destroy old container
            ↓
    Create new container
            ↓
    Use port 8081

The plan summary was:

    Plan: 1 to add, 0 to change, 1 to destroy.

This demonstrated that not every infrastructure change can be performed in place.

---

## 24. Applying the Configuration Change

The new desired state was applied using:

    terraform apply -auto-approve

Terraform destroyed the old container and created a replacement.

Result:

    Apply complete! Resources: 1 added, 0 changed, 1 destroyed.

The new container exposed:

    0.0.0.0:8081->80/tcp

The output also changed to:

    container_name = "terraform-nginx"
    container_port = 8081

---

## 25. Verify the Changed Infrastructure

The new port was tested with:

    curl -I http://localhost:8081

The response returned:

    HTTP/1.1 200 OK

The old port was also tested:

    curl http://localhost:8080

The result was:

    Failed to connect to localhost port 8080

This proved that the infrastructure had actually changed from port `8080` to port `8081`.

---

## 26. Final Plan Verification

After applying the configuration change, Terraform was run again with:

    terraform plan

Result:

    No changes. Your infrastructure matches the configuration.

This confirmed:

    Desired Configuration
            =
    Actual Infrastructure

No additional changes were required.

---

## 27. Terraform State Inspection

The managed resources were inspected using:

    terraform state list

The resources were:

    docker_container.nginx
    docker_image.nginx

The container state was inspected using:

    terraform state show docker_container.nginx

Important values included:

    name = "terraform-nginx"
    external = 8081
    internal = 80

This demonstrated that Terraform maintains information about the infrastructure it manages.

---

## 28. Terraform Dependency Graph

The dependency relationship was inspected using:

    terraform graph

The graph showed:

    docker_container.nginx
            ↓
    docker_image.nginx

The relationship exists because the container uses the image resource.

Terraform can therefore determine the correct resource dependency automatically.

---

## 29. Practical Workflow Completed

The practical Terraform workflow was:

    Create Configuration
            ↓
    terraform init
            ↓
    terraform fmt
            ↓
    terraform validate
            ↓
    terraform plan
            ↓
    terraform apply
            ↓
    Verify Docker Container
            ↓
    Verify Nginx
            ↓
    Add Variables
            ↓
    Add Outputs
            ↓
    Change Port
            ↓
    terraform plan
            ↓
    Resource Replacement
            ↓
    terraform apply
            ↓
    Verify New Port
            ↓
    terraform plan
            ↓
    No Changes
    
    
    
    
    # Step 26 — Terraform

## Part 3 — Practical Observations & Troubleshooting

---

## 30. What to Observe

Terraform learning was performed by observing the actual infrastructure changes rather than only running commands.

The important observations were:

| Stage | What to Observe | What It Proves |
|---|---|---|
| `terraform init` | Provider installed successfully | Terraform is ready to work |
| `terraform validate` | `Success! The configuration is valid.` | Configuration is syntactically valid |
| `terraform plan` | Planned resources and change symbols | Terraform understands the desired changes |
| `terraform apply` | Resources created/changed/destroyed | Desired state was applied |
| `docker ps` | Container status and ports | Infrastructure exists in Docker |
| `curl` | Nginx HTTP response | Application is reachable |
| `terraform output` | Output values | Useful state values are exposed |
| `terraform state list` | Managed resources | Terraform knows which resources it manages |
| Final `terraform plan` | `No changes` | Actual infrastructure matches configuration |

---

## 31. Initial Plan Observation

The first Terraform plan showed:

    Plan: 2 to add, 0 to change, 0 to destroy.

The two resources were:

    docker_image.nginx
    docker_container.nginx

The `+` symbol represented creation.

Therefore Terraform understood that the required infrastructure did not yet exist.

---

## 32. Initial Apply Observation

After applying the plan, Terraform reported:

    Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

The resources were:

    docker_image.nginx
    docker_container.nginx

The Docker container was then verified as:

    STATUS: Up

with:

    0.0.0.0:8080->80/tcp

This confirmed that Terraform successfully created the infrastructure described by the configuration.

---

## 33. Nginx Verification

The running container was tested with:

    curl http://localhost:8080

The response contained:

    Welcome to nginx!

This was an important observation because it proved more than just container creation.

The complete path worked:

    Host
      ↓
    Port 8080
      ↓
    Docker Container
      ↓
    Container Port 80
      ↓
    Nginx
      ↓
    HTTP Response

---

## 34. Output Observation

After outputs were added, the first plan showed:

    Changes to Outputs:
      + container_name = "terraform-nginx"
      + container_port = 8080

Before applying the output configuration, running:

    terraform output

returned:

    Warning: No outputs found

This happened because the output definitions had not yet been stored in Terraform state.

After applying the configuration, the output became available:

    container_name = "terraform-nginx"
    container_port = 8080

This demonstrated the relationship between Terraform configuration and Terraform state.

---

## 35. Configuration Change Observation

The external port was changed:

    8080 → 8081

Running:

    terraform plan

showed:

    external = 8080 -> 8081 # forces replacement

The plan summary was:

    Plan: 1 to add, 0 to change, 1 to destroy.

This means Terraform could not modify this container port in place.

Instead, it planned:

    Destroy old container
            ↓
    Create replacement container
            ↓
    Use port 8081

---

## 36. Replacement Observation

After applying the port change, Terraform reported:

    Apply complete! Resources: 1 added, 0 changed, 1 destroyed.

The new container showed:

    0.0.0.0:8081->80/tcp

The new HTTP endpoint returned:

    HTTP/1.1 200 OK

The old port returned a connection failure.

Therefore the configuration change was successfully applied.

---

## 37. State Observation

Terraform state was inspected using:

    terraform state list

The managed resources were:

    docker_container.nginx
    docker_image.nginx

The container state showed values including:

    name = "terraform-nginx"
    external = 8081
    internal = 80

This demonstrated that Terraform keeps track of the resources it manages.

---

## 38. Dependency Observation

The Terraform dependency graph showed the relationship between:

    docker_image.nginx

and:

    docker_container.nginx

Conceptually:

    docker_image.nginx
            ↓
    docker_container.nginx

The container requires the image.

Terraform therefore understands that the image must be available before the container can use it.

---

## 39. Drift Detection

A manual change was intentionally made outside Terraform.

The running container was stopped manually:

    docker stop terraform-nginx

Docker then showed:

    STATUS: Exited (0)

However, the Terraform configuration still described a running container.

Running:

    terraform plan

then produced:

    Plan: 1 to add, 0 to change, 0 to destroy.

Terraform proposed recreating the missing container.

This demonstrated infrastructure drift.

---

## 40. Drift Reconciliation

The desired state was restored with:

    terraform apply -auto-approve

Terraform created a replacement container.

Result:

    Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Docker then showed:

    STATUS: Up

and:

    0.0.0.0:8081->80/tcp

A final plan returned:

    No changes. Your infrastructure matches the configuration.

The drift was therefore successfully reconciled.

---

## 41. Desired State vs Actual State

The practical lab demonstrated the difference between desired and actual state.

| State | Example |
|---|---|
| Desired State | Terraform configuration requires `terraform-nginx` on port `8081` |
| Actual State | Docker container is actually running on port `8081` |
| Drift | Container is manually stopped |
| Reconciliation | Terraform recreates the container |

The important workflow was:

    Desired State
          ↓
    Compare
          ↓
    Actual State
          ↓
    Difference?
       ↙       ↘
     Yes        No
      ↓          ↓
    Correct    No Change

---

## 42. Troubleshooting — Output Not Available

### Problem

Running:

    terraform output

initially returned:

    Warning: No outputs found

### Cause

The output blocks existed in the configuration, but their values had not yet been saved to Terraform state.

### Fix

Apply the configuration:

    terraform apply -auto-approve

Then verify:

    terraform output

### Result

The expected values became available:

    container_name = "terraform-nginx"
    container_port = 8080

---

## 43. Troubleshooting — Resource Replacement

### Problem

Changing the external port produced:

    # docker_container.nginx must be replaced

and:

    external = 8080 -> 8081 # forces replacement

### Cause

The Docker container resource required replacement for this configuration change.

### Terraform Action

Terraform planned:

    1 to add
    0 to change
    1 to destroy

### Verification

After apply:

    0.0.0.0:8081->80/tcp

and:

    HTTP/1.1 200 OK

The replacement was successful.

---

## 44. Troubleshooting — Configuration Drift

### Problem

The container was manually stopped:

    docker stop terraform-nginx

Docker showed:

    Exited (0)

### Cause

The actual infrastructure no longer matched the Terraform-managed desired state.

### Detection

    terraform plan

Result:

    Plan: 1 to add, 0 to change, 0 to destroy.

### Fix

    terraform apply -auto-approve

### Verification

The container returned to:

    STATUS: Up

The final plan returned:

    No changes. Your infrastructure matches the configuration.

---

## 45. Important Terraform Plan Symbols

| Symbol | Meaning |
|---|---|
| `+` | Create |
| `-` | Destroy |
| `~` | Modify in place |
| `-/+` | Destroy and recreate |
| No symbol/change | No infrastructure change required |

The most important example from the lab was:

    -/+ destroy and then create replacement

This appeared when the container port changed from:

    8080 → 8081

---

## 46. Troubleshooting Workflow

The practical troubleshooting workflow can be summarized as:

    Check Configuration
            ↓
    terraform validate
            ↓
    Inspect State
            ↓
    terraform state list
            ↓
    Preview Changes
            ↓
    terraform plan
            ↓
    Understand the Plan
            ↓
    terraform apply
            ↓
    Verify Docker
            ↓
    Verify Application
            ↓
    terraform plan
            ↓
    No changes

This workflow helps prevent blindly applying infrastructure changes.

---

## 47. Practical Learning Result

The Terraform lab demonstrated the complete relationship between configuration, state, and infrastructure:

    Terraform Configuration
            ↓
      Desired State
            ↓
    terraform plan
            ↓
    Planned Changes
            ↓
    terraform apply
            ↓
    Real Infrastructure
            ↓
    Verification
            ↓
    Configuration Change / Drift
            ↓
    terraform plan
            ↓
    Reconciliation
            ↓
    No Changes

The practical lab therefore established the core Terraform operating model required for the next DevOps concepts.




# Step 26 — Terraform

## Part 4 — Final Verification, Security & Summary

---

## 48. Project Integration

Step 26 was completed as a Phase 2 supplementary lab.

The Terraform practice was intentionally kept separate from the main MERN project.

| Area | Path |
|---|---|
| Terraform Practice | `/home/afroza/Projects/devops-supplementary-labs/Step-26-Terraform/` |
| Documentation | `/home/afroza/Projects/devops-learning/Step-26-Terraform/` |
| Main MERN Project | `/home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack` |

The main MERN project was not modified during the Terraform lab.

The purpose of the separate lab was to understand Terraform as an Infrastructure as Code tool without unnecessarily introducing Terraform into the existing application project.

---

## 49. Relationship with Previous DevOps Steps

Terraform builds on the infrastructure knowledge developed in the previous steps.

The relationship is:

    Docker
        ↓
    Containerization

    Docker Compose
        ↓
    Multi-container local environment

    GitHub Actions
        ↓
    CI/CD automation

    Cloud Fundamentals
        ↓
    Understanding cloud infrastructure

    Terraform
        ↓
    Infrastructure as Code

    Kubernetes
        ↓
    Container Orchestration

These technologies solve different problems.

| Technology | Main Responsibility |
|---|---|
| Docker | Containerization |
| Docker Compose | Multi-container local environments |
| Terraform | Infrastructure provisioning and management |
| Kubernetes | Container orchestration |

---

## 50. Security Considerations

Although this lab used only local Docker infrastructure, Terraform security practices are important for real environments.

### 50.1 Protect Terraform State

The practical lab created:

    terraform.tfstate

Terraform state may contain infrastructure information and, depending on the configuration, sensitive values.

Therefore state should be protected and should not be exposed publicly.

Important practices:

    Protect state files
    Restrict access
    Do not expose sensitive state
    Use secure remote state management in production

---

### 50.2 Do Not Hard-Code Secrets

Credentials, passwords, API tokens, and cloud access keys should not be placed directly inside Terraform configuration.

Avoid storing real secrets directly in:

    *.tf

files.

Production environments should use secure credential and secret-management mechanisms.

---

### 50.3 Least Privilege

Terraform should receive only the permissions required to manage the required infrastructure.

The principle is:

    Minimum Required Permissions
            ↓
    Smaller Blast Radius
            ↓
    Better Security

---

### 50.4 Review Terraform Plan

Infrastructure changes should be reviewed before applying them.

Recommended workflow:

    terraform plan
          ↓
    Review Changes
          ↓
    terraform apply

The practical lab demonstrated why this is important when changing the external port.

Terraform clearly showed:

    external = 8080 -> 8081
    # forces replacement

This allowed the replacement behavior to be understood before applying it.

---

### 50.5 `-auto-approve`

The lab used:

    terraform apply -auto-approve

for controlled local practice.

The flag skips Terraform's interactive approval prompt.

For production infrastructure, automatic approval should be used only when appropriate review and safety controls already exist in the workflow.

---

### 50.6 Avoid Unnecessary Cloud Costs

The practical lab used local Docker infrastructure instead of a real cloud provider.

This avoided:

    Cloud credentials
    Cloud resource charges
    Accidental billing

This was appropriate for learning the Terraform fundamentals.

---

## 51. Final Verification

The Terraform installation was verified as:

    Terraform v1.15.8
    on linux_amd64

Docker was verified as:

    Docker 29.7.2
    Docker Engine 29.7.2

The Docker provider was successfully initialized:

    kreuzwerker/docker v3.9.0

---

## 52. Configuration Verification

The Terraform configuration was checked with:

    terraform validate

Result:

    Success! The configuration is valid.

This confirmed that the Terraform configuration could be successfully parsed and validated.

---

## 53. Infrastructure Verification

Terraform successfully managed:

    docker_image.nginx

and:

    docker_container.nginx

The Nginx container was successfully created and verified.

Initial port mapping:

    0.0.0.0:8080 -> 80/tcp

After the configuration change:

    0.0.0.0:8081 -> 80/tcp

---

## 54. Application Verification

Nginx was tested through HTTP.

The initial test returned the Nginx welcome page.

After changing the port, the new endpoint was tested with:

    curl -I http://localhost:8081

The response included:

    HTTP/1.1 200 OK

This verified that the new Terraform-managed infrastructure was functioning correctly.

The old port was tested and no longer served Nginx.

This confirmed that the configuration change had actually taken effect.

---

## 55. State Verification

Terraform state was inspected using:

    terraform state list

The managed resources were:

    docker_container.nginx
    docker_image.nginx

The container state showed important values including:

    name = "terraform-nginx"
    external = 8081
    internal = 80

This demonstrated that Terraform maintains information about infrastructure resources under its management.

---

## 56. Dependency Verification

The dependency relationship was inspected through:

    terraform graph

The practical relationship was:

    docker_image.nginx
            ↓
    docker_container.nginx

The container depends on the image.

Terraform therefore understands the resource relationship and can determine the appropriate order of operations.

---

## 57. Drift Verification

A manual infrastructure change was intentionally introduced:

    docker stop terraform-nginx

Docker then showed the container as:

    Exited (0)

Terraform was then used to detect the difference.

The plan showed:

    Plan: 1 to add, 0 to change, 0 to destroy.

Terraform therefore detected that the desired infrastructure required the container to exist again.

After applying:

    terraform apply -auto-approve

the container returned to:

    STATUS: Up

with:

    0.0.0.0:8081->80/tcp

The final plan returned:

    No changes. Your infrastructure matches the configuration.

This verified successful drift detection and reconciliation.

---

## 58. Terraform Destroy

The lab infrastructure was cleaned up using:

    terraform destroy -auto-approve

The purpose of `terraform destroy` is to remove infrastructure managed by the current Terraform configuration.

The configuration files remain available after destroy.

Conceptually:

    Terraform Configuration
            ↓
          remains

    Managed Infrastructure
            ↓
          removed

This demonstrates that configuration and infrastructure are separate concepts.

---

## 59. Final Terraform Lifecycle

The complete practical lifecycle demonstrated in this step was:

    Write Configuration
            ↓
    terraform init
            ↓
    terraform fmt
            ↓
    terraform validate
            ↓
    terraform plan
            ↓
    terraform apply
            ↓
    Verify Infrastructure
            ↓
    Change Configuration
            ↓
    terraform plan
            ↓
    Apply Replacement
            ↓
    Detect Manual Drift
            ↓
    Reconcile Infrastructure
            ↓
    terraform destroy

---

## 60. Key Lessons

### Infrastructure as Code

Infrastructure can be defined using configuration instead of being created manually.

### Declarative Model

Terraform describes the desired state and determines the actions required to reach that state.

### Provider

A provider connects Terraform to an external platform.

In this lab:

    Terraform
        ↓
    Docker Provider
        ↓
    Docker

### Resource

A resource represents an infrastructure object managed by Terraform.

The lab used:

    docker_image.nginx
    docker_container.nginx

### Variables

Variables allow configurable values to be separated from resource definitions.

Example:

    var.external_port

### Outputs

Outputs expose useful information from Terraform.

Example:

    container_name = "terraform-nginx"
    container_port = 8081

### State

Terraform state records information about managed infrastructure.

Example:

    terraform.tfstate

### Plan

`terraform plan` previews required infrastructure changes.

### Apply

`terraform apply` performs the planned infrastructure changes.

### Destroy

`terraform destroy` removes infrastructure managed by Terraform.

### Drift

When infrastructure is changed outside Terraform, the actual state can differ from the desired configuration.

Terraform can detect and reconcile this difference.

---

## 61. Important Differences

### Terraform Plan vs Apply

| `terraform plan` | `terraform apply` |
|---|---|
| Previews changes | Performs changes |
| Does not create infrastructure | Creates/updates/destroys infrastructure |
| Used for review | Used for execution |

### Configuration vs State

| Configuration | State |
|---|---|
| Defines desired infrastructure | Records managed infrastructure information |
| `.tf` files | `terraform.tfstate` |
| User-maintained | Terraform-maintained |

### Desired vs Actual State

| Desired State | Actual State |
|---|---|
| What Terraform configuration declares | What exists in infrastructure |
| Defined by `.tf` files | Exists in Docker/provider |
| Terraform uses it as the target | Terraform compares against it |

---

## 62. Final Result

### Step

    Step 26 — Terraform

### Status

    COMPLETE

### Phase

    Phase 2 — Supplementary Lab

### Practice Path

    /home/afroza/Projects/devops-supplementary-labs/Step-26-Terraform/

### Documentation Path

    /home/afroza/Projects/devops-learning/Step-26-Terraform/

### Technologies Used

    Terraform v1.15.8
    Docker 29.7.2
    Docker Engine 29.7.2
    kreuzwerker/docker v3.9.0
    nginx:alpine

### Practical Concepts Completed

    Infrastructure as Code
    Declarative Configuration
    Terraform CLI
    Provider
    Resource
    Variables
    Outputs
    Configuration Files
    terraform init
    terraform fmt
    terraform validate
    terraform plan
    terraform apply
    terraform output
    Terraform State
    Resource Dependencies
    Resource Replacement
    Configuration Changes
    Drift Detection
    Drift Reconciliation
    terraform destroy
    Infrastructure Verification
    Troubleshooting
    Security Considerations

---

## 63. Final Learning Outcome

The core Terraform model is now understood through practical work:

    Manual Infrastructure
            ↓
    Infrastructure as Code
            ↓
    Terraform Configuration
            ↓
    terraform plan
            ↓
    Review Changes
            ↓
    terraform apply
            ↓
    Infrastructure
            ↓
    Verify
            ↓
    Detect Changes / Drift
            ↓
    Reconcile
            ↓
    terraform destroy

Terraform was therefore learned as an infrastructure provisioning and management tool rather than only as a collection of CLI commands.

---

# Step 26 — Final Status: COMPLETE
