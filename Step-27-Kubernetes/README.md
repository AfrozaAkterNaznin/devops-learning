# Step 27 — Kubernetes

## 1. Objective

The objective of this step is to understand the fundamentals of Kubernetes and gain practical experience with a local Kubernetes cluster using Minikube.

The main goals of this step were:

- Understand Kubernetes and its role in container orchestration
- Understand the basic Kubernetes architecture
- Create and manage Pods
- Expose Pods using a Kubernetes Service
- Understand Labels and Selectors
- Create a Deployment
- Run multiple replicas of an application
- Understand Kubernetes self-healing
- Perform a Rolling Update
- Verify the desired state of the Kubernetes workload
- Understand the relationship between Docker, Kubernetes, and Nginx

The practical work was performed using an Nginx container running inside a local Kubernetes cluster.

---

## 2. Environment

### 2.1 Operating System

| Component | Value |
|---|---|
| Host Environment | Ubuntu running inside VirtualBox |
| Architecture | amd64 |
| Kubernetes Tool | kubectl |
| Local Cluster Tool | Minikube |
| Container Runtime | Docker |
| Application | Nginx |
| Practice Directory | `/home/afroza/Projects/devops-supplementary-labs/Step-27-Kubernetes` |
| Documentation Directory | `/home/afroza/Projects/devops-learning/Step-27-Kubernetes` |

### 2.2 Tool Versions

| Tool | Version / Information |
|---|---|
| kubectl | v1.36.3 |
| Kustomize | v5.8.1 |
| Minikube | v1.38.1 |
| Kubernetes Cluster | v1.35.1 |
| Container Runtime | Docker |
| Nginx Image | `nginx:alpine` |
| Updated Nginx Image | `nginx:1.27-alpine` |

### 2.3 Local Kubernetes Environment

The Kubernetes cluster was created locally using Minikube.

Minikube was configured to use the Docker driver.

The final cluster verification showed:

| Component | Observed State |
|---|---|
| Minikube | Running |
| Kubernetes Context | `minikube` |
| Node | `minikube` |
| Node Status | Ready |
| Node Role | control-plane |
| Kubernetes Version | v1.35.1 |
| Container Runtime | Docker |
| Cluster Network | `192.168.49.2` |

The cluster was therefore successfully available for Kubernetes practice.

---

## 3. Short Theory

### 3.1 What is Kubernetes?

Kubernetes is a container orchestration platform.

Docker can create and run containers, but running containers manually becomes difficult when an application requires:

- Multiple instances
- Automatic recovery
- Scaling
- Service discovery
- Rolling updates
- Desired-state management

Kubernetes provides these orchestration capabilities.

A simple way to understand the relationship is:

Docker:

    Runs containers

Kubernetes:

    Manages containerized applications

---

### 3.2 What is a Kubernetes Cluster?

A Kubernetes Cluster is the complete Kubernetes environment where workloads run.

A basic cluster contains:

| Component | Role |
|---|---|
| Control Plane | Manages the cluster |
| Node | Runs workloads |
| Pod | Smallest deployable Kubernetes unit |
| Container | Runs the actual application |

In this practice environment, Minikube created a local single-node Kubernetes cluster.

---

### 3.3 What is Minikube?

Minikube is a tool used to run a Kubernetes cluster locally.

Instead of requiring a real cloud Kubernetes cluster, Minikube allows Kubernetes concepts to be practiced on a local machine.

In this step:

    Ubuntu
       ↓
    Minikube
       ↓
    Kubernetes Cluster
       ↓
    minikube Node
       ↓
    Pods
       ↓
    Nginx Container

Minikube was therefore used as the local Kubernetes environment for this step.

---

### 3.4 What is kubectl?

`kubectl` is the command-line tool used to communicate with the Kubernetes cluster.

It allows an administrator or developer to:

- Create Kubernetes resources
- View Pods
- View Services
- View Deployments
- Scale workloads
- Update workloads
- Delete resources
- Inspect resource status

The basic relationship is:

    kubectl
       ↓
    Kubernetes API Server
       ↓
    Kubernetes Cluster

---

### 3.5 What is a Pod?

A Pod is the smallest deployable unit in Kubernetes.

A Pod can contain one or more containers.

In this step, an Nginx container was initially run inside a Pod:

    nginx-pod
        │
        └── nginx container
                │
                └── nginx:alpine

The Pod itself is managed by Kubernetes.

---

### 3.6 What is a Service?

A Kubernetes Service provides a stable network endpoint for accessing Pods.

Pods can be recreated and their IP addresses can change.

Therefore, clients should not normally depend directly on an individual Pod IP.

A Service provides a stable way to reach the application.

In this practice:

    Client
       ↓
    nginx-service
       ↓
    nginx Pod
       ↓
    Nginx container :80

A `NodePort` Service was used to expose Nginx outside the cluster.

---

### 3.7 What are Labels and Selectors?

Labels are key-value pairs attached to Kubernetes resources.

Example:

    app=nginx

Selectors are used by Kubernetes resources such as Services to find resources with matching labels.

Example:

    Service selector
        ↓
    app=nginx
        ↓
    Pod label
        ↓
    app=nginx

This is how the Service identifies the correct Pod.

---

### 3.8 What is a Deployment?

A Deployment manages application Pods.

Instead of manually creating and maintaining multiple Pods, a Deployment defines the desired number of replicas and Kubernetes maintains that desired state.

For example:

    Deployment
         ↓
    ReplicaSet
         ↓
    3 Pods
         ↓
    Nginx containers

In this step, the Deployment was configured with 3 replicas.

---

### 3.9 What is a ReplicaSet?

A ReplicaSet ensures that the required number of Pod replicas exist.

For example:

    Desired replicas = 3

If only 2 Pods are running:

    ReplicaSet
        ↓
    Creates another Pod
        ↓
    Total = 3

This provides basic self-healing for Pods managed by a Deployment.

---

## 4. Architecture / Workflow

### 4.1 Kubernetes Practice Architecture

The final practical architecture can be represented as:

    User
      │
      │ kubectl commands
      ↓
    Kubernetes API Server
      │
      ├──────────────→ Deployment
      │                    │
      │                    ↓
      │                 ReplicaSet
      │                    │
      │              ┌─────┼─────┐
      │              ↓     ↓     ↓
      │            Pod   Pod   Pod
      │              │     │     │
      │              └─────┼─────┘
      │                    │
      ↓                    ↓
    Service ─────────→ Nginx Containers
                             │
                             ↓
                       nginx:alpine
                       / nginx:1.27-alpine

---

### 4.2 Practical Workflow

The practical workflow followed in this step was:

    1. Install and verify kubectl
             ↓
    2. Install and verify Minikube
             ↓
    3. Start local Kubernetes cluster
             ↓
    4. Verify Kubernetes context
             ↓
    5. Verify cluster and node
             ↓
    6. Create nginx Pod
             ↓
    7. Verify Pod status
             ↓
    8. Add nginx label
             ↓
    9. Create NodePort Service
             ↓
    10. Verify Service and Endpoint
             ↓
    11. Test Nginx through Service
             ↓
    12. Create Deployment
             ↓
    13. Scale Deployment to 3 replicas
             ↓
    14. Delete one Pod manually
             ↓
    15. Observe automatic Pod recovery
             ↓
    16. Update Nginx image
             ↓
    17. Observe Rolling Update
             ↓
    18. Verify new Pods and ReplicaSet
             ↓
    19. Confirm final Deployment state

---

### 4.3 Docker vs Kubernetes vs Nginx

These three technologies have different responsibilities.

| Technology | Main Role | In This Practice |
|---|---|---|
| Docker | Container runtime / container management | Runs the container |
| Kubernetes | Container orchestration | Manages Pods, replicas, Services and Deployments |
| Nginx | Web server | Serves the web content |
| Minikube | Local Kubernetes environment | Provides the local Kubernetes cluster |
| kubectl | Kubernetes CLI | Communicates with the cluster |

The relationship is:

    Kubernetes
        │
        └── manages workload
                │
                └── Pod
                     │
                     └── Container
                           │
                           └── Nginx

Docker provides the container runtime, Kubernetes manages the workload, and Nginx is the application running inside the container.

---

### 4.4 Final Conceptual Flow

The overall concept learned in this step is:

    Application
        ↓
    Container Image
        ↓
    Pod
        ↓
    Deployment
        ↓
    ReplicaSet
        ↓
    Multiple Pods
        ↓
    Service
        ↓
    Network Access

Kubernetes continuously compares the desired state defined by the configuration with the actual state of the cluster and takes action when the two differ.



## 5. Commands

### 5.1 Minikube and Cluster Commands

    minikube version

    minikube start --driver=docker --memory=3072

    minikube status

    minikube ip

    kubectl version --client

    kubectl config current-context

    kubectl cluster-info

    kubectl get nodes -o wide

These commands were used to verify the local Kubernetes environment and cluster connectivity.

---

### 5.2 Pod Commands

    kubectl apply -f nginx-pod.yaml

    kubectl get pods

    kubectl get pod nginx-pod -o wide

    kubectl describe pod nginx-pod

    kubectl logs nginx-pod

    kubectl label pod nginx-pod app=nginx

The Pod commands were used to create, inspect, label, and verify the Nginx Pod.

---

### 5.3 Service Commands

    kubectl expose pod nginx-pod --name=nginx-service --type=NodePort --port=80

    kubectl get services

    kubectl describe service nginx-service

    minikube service nginx-service --url

    curl http://<service-url>

The Service commands were used to expose the Nginx Pod and verify external access.

---

### 5.4 Deployment Commands

    kubectl create deployment nginx-deployment --image=nginx:alpine

    kubectl get deployments

    kubectl get replicasets

    kubectl get pods

    kubectl describe deployment nginx-deployment

The Deployment commands were used to create and inspect a Kubernetes Deployment.

---

### 5.5 Scaling Commands

    kubectl scale deployment nginx-deployment --replicas=3

    kubectl get deployment nginx-deployment

    kubectl get pods

Scaling was used to increase the desired number of Nginx Pods from one to three.

---

### 5.6 Self-Healing Verification Commands

    kubectl get pods

    kubectl delete pod <deployment-pod-name>

    kubectl get pods

    kubectl rollout status deployment/nginx-deployment

The Pod deletion was intentional. It was used to verify that the Deployment and ReplicaSet automatically recreate the missing Pod.

---

### 5.7 Rolling Update Commands

    kubectl set image deployment/nginx-deployment nginx=nginx:1.27-alpine

    kubectl rollout status deployment/nginx-deployment

    kubectl get deployment nginx-deployment

    kubectl get replicasets

    kubectl get pods

The image update was used to demonstrate a Kubernetes Rolling Update.

---

## 6. Command Explanation

### 6.1 `minikube start`

    minikube start --driver=docker --memory=3072

Starts a local Kubernetes cluster using Minikube.

In this practice environment, the Docker driver was used.

Minikube creates the Kubernetes control-plane environment locally so that Kubernetes workloads can be practiced without using a cloud provider.

---

### 6.2 `kubectl get`

    kubectl get pods

    kubectl get services

    kubectl get deployments

    kubectl get replicasets

The `get` command displays the current state of Kubernetes resources.

It is one of the most frequently used commands for checking whether resources are running correctly.

---

### 6.3 `kubectl describe`

    kubectl describe pod nginx-pod

    kubectl describe service nginx-service

    kubectl describe deployment nginx-deployment

The `describe` command provides detailed information about a Kubernetes resource.

It is especially useful for troubleshooting because it can show:

- Current configuration
- Status
- Conditions
- Events
- Scheduling information
- Container state
- Related resource information

---

### 6.4 `kubectl logs`

    kubectl logs nginx-pod

Displays the logs generated by the container inside the Pod.

For the Nginx Pod, the logs showed Nginx startup and worker process information.

Logs help verify whether the application started successfully.

---

### 6.5 `kubectl label`

    kubectl label pod nginx-pod app=nginx

Adds the label:

    app=nginx

to the Pod.

Labels are important because Kubernetes Services and other resources can use selectors to identify the correct Pods.

---

### 6.6 `kubectl expose`

    kubectl expose pod nginx-pod --name=nginx-service --type=NodePort --port=80

Creates a Service for the Pod.

The Service provides a stable network endpoint for accessing the application.

The `NodePort` type exposes the Service through a port on the Kubernetes node.

---

### 6.7 `kubectl create deployment`

    kubectl create deployment nginx-deployment --image=nginx:alpine

Creates a Deployment using the specified Nginx container image.

The Deployment then creates and manages a ReplicaSet, which manages the application Pods.

Relationship:

    Deployment
        ↓
    ReplicaSet
        ↓
    Pods
        ↓
    Nginx containers

---

### 6.8 `kubectl scale`

    kubectl scale deployment nginx-deployment --replicas=3

Changes the desired number of replicas to three.

Kubernetes then attempts to maintain:

    Desired = 3 Pods

If fewer than three Pods are available, Kubernetes creates additional Pods.

---

### 6.9 `kubectl delete pod`

    kubectl delete pod <deployment-pod-name>

Deletes a Pod manually.

This was intentionally performed to test Kubernetes self-healing.

The Deployment's ReplicaSet detected that the actual number of Pods was below the desired number and created a replacement Pod.

---

### 6.10 `kubectl set image`

    kubectl set image deployment/nginx-deployment nginx=nginx:1.27-alpine

Changes the container image used by the Deployment.

The Deployment then performs a Rolling Update.

The old Pods are gradually replaced by new Pods using the new image.

---

### 6.11 `kubectl rollout status`

    kubectl rollout status deployment/nginx-deployment

Displays the progress of a Deployment rollout.

A successful result such as:

    deployment "nginx-deployment" successfully rolled out

confirms that the update completed successfully.

---

## 7. Important Flags

| Command / Flag | Meaning |
|---|---|
| `--driver=docker` | Uses Docker as the Minikube driver |
| `--memory=3072` | Allocates approximately 3072 MiB of memory to Minikube |
| `-f` | Uses a manifest file as input |
| `-o wide` | Displays additional resource information |
| `--name` | Specifies a resource name |
| `--type=NodePort` | Creates a Service accessible through a node port |
| `--port=80` | Defines the Service port |
| `--replicas=3` | Sets the desired number of replicas |
| `--image` | Specifies the container image |
| `--url` | Displays the accessible Minikube Service URL |

### Important Observation

The Minikube version originally installed on the system was very old and did not support the modern `--driver` syntax.

The old version was removed and a current Minikube binary was installed.

After the update, the command:

    minikube start --driver=docker

worked successfully.

This was an important practical lesson: command syntax depends on the installed tool version.

---

## 8. Command Variations

### 8.1 View Pods

Basic:

    kubectl get pods

With additional information:

    kubectl get pods -o wide

For all namespaces:

    kubectl get pods -A

---

### 8.2 View Services

Basic:

    kubectl get services

Short form:

    kubectl get svc

---

### 8.3 View Deployments

Basic:

    kubectl get deployments

Short form:

    kubectl get deploy

---

### 8.4 View ReplicaSets

Basic:

    kubectl get replicasets

Short form:

    kubectl get rs

---

### 8.5 Inspect Resources

Basic:

    kubectl describe pod nginx-pod

The same pattern can be used with other resources:

    kubectl describe service nginx-service

    kubectl describe deployment nginx-deployment

---

### 8.6 View Logs

Basic:

    kubectl logs nginx-pod

Follow logs continuously:

    kubectl logs -f nginx-pod

The `-f` option follows the log output and keeps the command attached while new log entries appear.

---

### 8.7 Deployment Scaling

Scale to three replicas:

    kubectl scale deployment nginx-deployment --replicas=3

Scale to five replicas:

    kubectl scale deployment nginx-deployment --replicas=5

Scale back to one replica:

    kubectl scale deployment nginx-deployment --replicas=1

The same Deployment can therefore be scaled according to workload requirements.

---

### 8.8 Check Rollout

    kubectl rollout status deployment/nginx-deployment

View rollout history:

    kubectl rollout history deployment/nginx-deployment

These commands are useful for understanding Deployment update history and rollout progress.

---

## 9. Configuration

### 9.1 Pod Manifest

The initial Nginx Pod was defined using a Kubernetes YAML manifest.

Important configuration:

| Setting | Value |
|---|---|
| `apiVersion` | `v1` |
| `kind` | `Pod` |
| Pod name | `nginx-pod` |
| Container name | `nginx` |
| Image | `nginx:alpine` |
| Container Port | `80` |

The basic resource relationship was:

    Pod
      │
      └── Container
             │
             ├── Name: nginx
             ├── Image: nginx:alpine
             └── Port: 80

---

### 9.2 Pod Label

The Pod was given the label:

    app=nginx

This label was required for the Service selector.

The relationship was:

    Pod label
        ↓
    app=nginx

    Service selector
        ↓
    app=nginx

Because the values matched, the Service could identify the Nginx Pod.

---

### 9.3 Service Configuration

The Nginx Service used:

| Configuration | Value |
|---|---|
| Name | `nginx-service` |
| Type | `NodePort` |
| Service Port | `80` |
| Target Port | `80` |
| NodePort | `31270` |
| Selector | `app=nginx` |

The resulting access path was:

    Client
       ↓
    NodePort 31270
       ↓
    nginx-service
       ↓
    Pod port 80
       ↓
    Nginx

The verified Service URL was:

    http://192.168.49.2:31270

A successful HTTP response confirmed that traffic reached the Nginx application.

---

### 9.4 Deployment Configuration

The Deployment used:

| Configuration | Value |
|---|---|
| Deployment | `nginx-deployment` |
| Image | `nginx:alpine` initially |
| Final Image | `nginx:1.27-alpine` |
| Desired Replicas | `3` |
| Strategy | `RollingUpdate` |

The Deployment maintained three Nginx Pods after scaling.

---

### 9.5 Rolling Update Configuration

The image was updated from:

    nginx:alpine

to:

    nginx:1.27-alpine

Kubernetes created a new ReplicaSet for the updated image and gradually replaced the Pods belonging to the old ReplicaSet.

Final state:

    Old ReplicaSet
        Desired: 0
        Current: 0
        Ready: 0

    New ReplicaSet
        Desired: 3
        Current: 3
        Ready: 3

This demonstrated Kubernetes desired-state management and Rolling Update behavior.


## 10. Practical Work

### 10.1 Local Kubernetes Cluster Setup

The first practical task was to create a local Kubernetes cluster using Minikube.

Initially, the system contained an outdated Minikube installation that did not support the modern driver syntax.

The old installation was removed and a current Minikube binary was installed.

The final environment used the Docker driver.

The cluster was successfully started with:

    minikube start --driver=docker --memory=3072

The cluster verification showed:

    Current context: minikube

    Kubernetes control plane is running

    Node:
    minikube    Ready    control-plane

This confirmed that the local Kubernetes cluster was operational.

---

### 10.2 Create an Nginx Pod

An Nginx Pod was created using a Kubernetes manifest.

The Pod used:

    image: nginx:alpine

The initial Pod state was:

    STATUS: ContainerCreating

This was expected because Kubernetes was creating the container and pulling the required image.

After waiting for the Pod to become ready, the final state was:

    READY: 1/1
    STATUS: Running
    RESTARTS: 0

This confirmed that the Nginx container was running successfully inside the Pod.

---

### 10.3 Inspect the Pod

The Pod was inspected using:

    kubectl describe pod nginx-pod

The output showed important information such as:

- Pod name
- Namespace
- Node
- Container image
- Container port
- Container state
- Readiness state
- Mounted service-account information
- Kubernetes events

The events showed that the Pod was successfully scheduled and the Nginx image was pulled.

The Pod therefore moved through the expected lifecycle:

    Pending
       ↓
    ContainerCreating
       ↓
    Running

---

### 10.4 Verify Nginx Logs

The Nginx container logs were checked.

The logs showed:

    Configuration complete; ready for start up

and Nginx worker processes were started.

The logs also identified:

    nginx/1.31.3

This confirmed that the Nginx application successfully started inside the Kubernetes Pod.

---

### 10.5 Add a Label

The Pod was labeled:

    app=nginx

The label was verified with the Pod listing.

This was necessary because the Kubernetes Service used the same label as its selector.

The relationship became:

    Pod
      label: app=nginx

            ↓

    Service
      selector: app=nginx

---

### 10.6 Create a NodePort Service

A NodePort Service named:

    nginx-service

was created to expose the Nginx Pod.

The Service details showed:

    Type: NodePort
    Port: 80/TCP
    NodePort: 31270/TCP
    Selector: app=nginx

The Service Endpoint was:

    10.244.0.4:80

This confirmed that the Service had successfully discovered the Nginx Pod.

---

### 10.7 Test Nginx Through the Service

The Service URL was:

    http://192.168.49.2:31270

An HTTP request was sent to the Service.

The response was:

    HTTP/1.1 200 OK

The response identified:

    Server: nginx/1.31.3

The response body contained:

    Welcome to nginx!

This confirmed the complete network path:

    Client
       ↓
    NodePort
       ↓
    nginx-service
       ↓
    Pod
       ↓
    Nginx container
       ↓
    HTTP 200 OK

---

### 10.8 Create a Deployment

An Nginx Deployment named:

    nginx-deployment

was created.

Initially, the Deployment was configured with one replica.

The Deployment created a ReplicaSet:

    nginx-deployment-7d74c5f744

The ReplicaSet was responsible for maintaining the desired Pod count.

---

### 10.9 Scale the Deployment

The Deployment was scaled from one replica to three replicas.

The desired state became:

    replicas = 3

After the rollout completed, the Deployment showed:

    READY: 3/3
    UP-TO-DATE: 3
    AVAILABLE: 3

Three Nginx Pods were running.

This demonstrated Kubernetes horizontal scaling at the Pod level.

---

### 10.10 Test Self-Healing

One Deployment-managed Pod was manually deleted.

Immediately after deletion, the Deployment temporarily had fewer than the desired number of Pods.

The ReplicaSet detected the difference between:

    Desired state = 3

and:

    Actual running Pods < 3

A replacement Pod was automatically created.

After waiting for recovery, the final state again showed:

    READY: 3/3
    AVAILABLE: 3

This demonstrated Kubernetes self-healing.

---

### 10.11 Perform a Rolling Update

The Deployment image was changed from:

    nginx:alpine

to:

    nginx:1.27-alpine

The rollout status showed that new replicas were gradually updated.

The output included messages indicating:

    1 out of 3 new replicas have been updated

then:

    2 out of 3 new replicas have been updated

and finally:

    deployment "nginx-deployment" successfully rolled out

The old ReplicaSet was reduced to zero replicas while the new ReplicaSet maintained three replicas.

Final new Pods were running with:

    nginx:1.27-alpine

This demonstrated a Kubernetes Rolling Update.

---

## 11. What to Observe

The most important learning in this step comes from observing Kubernetes state transitions rather than only running commands.

### 11.1 Cluster State

After starting Minikube, observe:

| Item | Expected Observation |
|---|---|
| Context | `minikube` |
| Node | `minikube` |
| Node Status | `Ready` |
| Role | `control-plane` |
| Cluster API | Running |

If the context is not `minikube`, kubectl may not be connected to the intended cluster.

---

### 11.2 Pod Creation

Immediately after creating a Pod, it may show:

    ContainerCreating

This does not automatically mean there is an error.

Kubernetes may still be:

- Scheduling the Pod
- Pulling the image
- Creating the container
- Preparing networking

After successful startup, observe:

    READY: 1/1
    STATUS: Running

---

### 11.3 Pod Readiness

`1/1` means one container is defined and one container is ready.

For example:

    nginx-pod    1/1    Running

means the Nginx container inside the Pod is ready.

---

### 11.4 Service Endpoint

The Service should have an Endpoint such as:

    10.244.0.4:80

This is important.

It proves that the Service selector successfully found the matching Pod.

If there is no Endpoint, investigate:

- Pod labels
- Service selector
- Pod readiness
- Namespace

---

### 11.5 HTTP Response

The Service test returned:

    HTTP/1.1 200 OK

This proves that the request successfully reached the Nginx application.

The important verification chain is:

    Service exists
        ↓
    Endpoint exists
        ↓
    Request reaches Service
        ↓
    Nginx responds
        ↓
    HTTP 200 OK

---

### 11.6 Deployment Scaling

After scaling to three replicas, observe:

    READY: 3/3
    UP-TO-DATE: 3
    AVAILABLE: 3

This indicates that the Deployment successfully reached its desired state.

---

### 11.7 Self-Healing

After manually deleting one Pod, observe that Kubernetes creates a replacement Pod.

The replacement Pod receives a different Pod name.

The important observation is:

    Pod deleted
        ↓
    Replica count temporarily decreases
        ↓
    ReplicaSet detects difference
        ↓
    New Pod created
        ↓
    New Pod becomes Running
        ↓
    Desired replicas restored

---

### 11.8 Rolling Update

During the image update, observe:

    Old ReplicaSet
        ↓
    gradually reduced

and:

    New ReplicaSet
        ↓
    gradually increased

At the end:

    Old ReplicaSet = 0

    New ReplicaSet = 3

This proves that the Deployment successfully switched to the new image.

---

## 12. Actual Output Meaning

### 12.1 `Ready`

Example:

    minikube    Ready

Meaning:

The Kubernetes node is available and can run workloads.

---

### 12.2 `1/1 Running`

Example:

    nginx-pod    1/1    Running

Meaning:

- One container is expected
- One container is ready
- The Pod is currently running

---

### 12.3 `ContainerCreating`

Example:

    nginx-pod    0/1    ContainerCreating

Meaning:

The Pod has been scheduled but the container is still being prepared.

It is a temporary lifecycle state.

---

### 12.4 Service `NodePort`

Example:

    nginx-service
    NodePort
    80:31270/TCP

Meaning:

- Application listens on port `80`
- Kubernetes exposes it through NodePort `31270`
- External requests can reach the Service through that NodePort

---

### 12.5 Service Endpoint

Example:

    10.244.0.4:80

Meaning:

The Service currently has a backend Pod available at that Pod IP and port.

This is evidence that the Service selector matched the Pod.

---

### 12.6 `HTTP/1.1 200 OK`

Meaning:

The HTTP request succeeded.

In this practice, it proved that Nginx was reachable through the Kubernetes Service.

---

### 12.7 `READY 3/3`

Example:

    nginx-deployment    3/3

Meaning:

The Deployment wants three replicas and all three are ready.

---

### 12.8 `DESIRED / CURRENT / READY`

Example:

    DESIRED    CURRENT    READY
       3          3          3

Meaning:

| Field | Meaning |
|---|---|
| Desired | Number of replicas Kubernetes should maintain |
| Current | Number of replicas currently created |
| Ready | Number of replicas currently ready |

When all three values are `3`, the Deployment has reached the desired state.

---

### 12.9 New ReplicaSet After Image Update

Example:

    nginx-deployment-7fc65ff6cf

This different ReplicaSet name indicates that the Deployment created a new ReplicaSet for the updated Pod template.

The final state was:

    Old ReplicaSet
    Desired: 0
    Current: 0
    Ready: 0

    New ReplicaSet
    Desired: 3
    Current: 3
    Ready: 3

This means the new version completely replaced the old version.

---

### 12.10 `successfully rolled out`

Example:

    deployment "nginx-deployment" successfully rolled out

Meaning:

The Deployment update completed successfully and the new desired state was reached.

---

## 13. Differences

### 13.1 Docker vs Kubernetes vs Nginx

These technologies are related but they do completely different jobs.

| Feature | Docker | Kubernetes | Nginx |
|---|---|---|---|
| What is it? | Container platform/runtime | Container orchestration platform | Web server / reverse proxy |
| Main job | Build and run containers | Manage containerized workloads | Serve web content and handle HTTP traffic |
| Manages Pods? | No | Yes | No |
| Manages replicas? | Not by itself | Yes | No |
| Self-healing | Limited/manual | Yes | No |
| Scaling | Mainly manual/container based | Built-in orchestration support | Application/server level |
| Service discovery | Basic Docker networking | Kubernetes Services/DNS | Not its primary role |
| Rolling updates | Not its main purpose | Yes, through Deployments | Not its main purpose |
| Runs the application? | Runs the container | Schedules/manages the workload | Runs as the web application inside a container |
| Role in this step | Container runtime | Managed Pods, Services and Deployments | Web server |
| Example | `nginx:1.27-alpine` container | `nginx-deployment` | Nginx HTTP server |

Simple mental model:

    Docker
      ↓
    "Run the container"

    Kubernetes
      ↓
    "Manage the containers/workload"

    Nginx
      ↓
    "Serve the web application"

---

### 13.2 Pod vs Deployment

| Feature | Pod | Deployment |
|---|---|---|
| Purpose | Runs application container(s) | Manages application Pods |
| Replica management | No | Yes |
| Self-healing | Not by itself | Yes, through ReplicaSet |
| Scaling | Manual | Built-in through replicas |
| Rolling update | No | Yes |
| Typical use | Small/basic workload unit | Production-style workload management |

A Pod is the workload unit.

A Deployment is the controller that manages the desired state of replicated Pods.

---

### 13.3 Service vs Pod

| Feature | Pod | Service |
|---|---|---|
| Main purpose | Runs application | Provides stable network access |
| Has container | Yes | No |
| Has Pod IP | Yes | Has a stable Service IP |
| IP stability | Can change when Pod is recreated | Stable within cluster |
| Selects Pods | No | Yes |
| External access | Not normally preferred directly | Can expose application |

---

### 13.4 Deployment vs ReplicaSet

| Feature | Deployment | ReplicaSet |
|---|---|---|
| Main purpose | Manage application lifecycle | Maintain desired Pod count |
| Scaling | Yes | Maintains replica count |
| Rolling update | Yes | No, not its primary role |
| Creates ReplicaSet | Yes | N/A |
| Creates Pods | Indirectly | Yes |

The normal hierarchy is:

    Deployment
        ↓
    ReplicaSet
        ↓
    Pods

---

## 14. Troubleshooting

### 14.1 Minikube Driver Error

#### Problem

The old Minikube installation returned:

    unknown flag: --driver

#### Cause

The installed Minikube version was extremely old:

    minikube version: v0.8.0

The modern `--driver` syntax was not supported by that version.

#### Fix

The old Snap-based Minikube installation was removed.

A current Minikube binary was installed under:

    /usr/local/bin/minikube

The final version was:

    minikube version: v1.38.1

The Docker driver then worked successfully.

---

### 14.2 No Kubernetes Context

#### Problem

Kubectl initially showed:

    current-context is not set

#### Cause

No Kubernetes cluster was running or configured as the active kubectl context.

#### Fix

The Minikube cluster was started successfully.

After startup:

    kubectl config current-context

returned:

    minikube

---

### 14.3 Kubernetes API Connection Refused

#### Problem

Kubectl initially returned:

    The connection to the server localhost:8080 was refused

#### Cause

There was no active Kubernetes API server available because the Minikube cluster had not started successfully.

#### Fix

The Minikube installation and driver configuration were corrected.

After the cluster started successfully, `kubectl cluster-info` showed that the Kubernetes control plane was running.

---

### 14.4 Service Creation Failed

#### Problem

The first Service creation attempt failed with:

    the pod has no labels and cannot be exposed

#### Cause

The Pod did not have a label that could be used by the Service selector.

#### Fix

The Pod was labeled:

    app=nginx

The Service was then created using the matching selector.

After that, the Service successfully discovered the Pod.

---

### 14.5 Service Not Found

#### Problem

The initial Service verification returned:

    services "nginx-service" not found

#### Cause

The Service had not been successfully created because the Pod-label requirement had failed.

#### Fix

The Pod was labeled correctly and the Service was created again.

Final verification showed:

    nginx-service
    NodePort
    80:31270/TCP

---

### 14.6 Pod Temporarily Shows `ContainerCreating`

#### Problem

The Pod initially showed:

    0/1    ContainerCreating

#### Cause

Kubernetes was still preparing the container and pulling the Nginx image.

#### Fix

Wait for the Pod to complete initialization.

Final state:

    1/1    Running

This was normal startup behavior rather than a permanent failure.

---

### 14.7 Deployment Temporarily Shows `0/3`

During scaling or rollout, the Deployment temporarily showed states such as:

    READY: 0/3

or individual Pods showed:

    ContainerCreating

#### Cause

New Pods were still being created and initialized.

#### Fix

Wait for the Deployment rollout to complete and verify:

    READY: 3/3
    UP-TO-DATE: 3
    AVAILABLE: 3

The final state confirmed successful recovery.

---

### 14.8 Important Troubleshooting Principle

Kubernetes troubleshooting should follow the resource relationship instead of looking at one command in isolation.

Recommended investigation flow:

    Cluster
       ↓
    Node
       ↓
    Deployment
       ↓
    ReplicaSet
       ↓
    Pod
       ↓
    Container
       ↓
    Service
       ↓
    Endpoint
       ↓
    Application response

This makes it easier to identify where the failure actually occurs.


## 15. Project Integration

Step 27 was intentionally practiced as a separate Phase 2 supplementary lab.

The main MERN project was not modified during this Kubernetes step.

### Phase 1 Main Project

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

Kubernetes was not directly integrated into this project during this step because the purpose of Step 27 was to understand Kubernetes fundamentals independently.

### Phase 2 Kubernetes Practice

    /home/afroza/Projects/devops-supplementary-labs/Step-27-Kubernetes

This directory was used for all Kubernetes practical work.

The concepts learned here can later be applied to real applications such as:

    MERN Application
        ↓
    Docker Containers
        ↓
    Kubernetes Pods
        ↓
    Deployments
        ↓
    Services
        ↓
    External Access

The separation between the supplementary lab and the main project was maintained intentionally.

---

## 16. Security Considerations

Even though this was a local learning environment, several production-oriented security concepts are important.

### 16.1 Do Not Expose Services Unnecessarily

The `NodePort` Service was used for learning and local testing.

In production, services should not automatically be exposed externally unless there is a clear requirement.

---

### 16.2 Container Images

Container images should come from trusted sources.

The practice used:

    nginx:alpine

and later:

    nginx:1.27-alpine

Using explicit versions is preferable to relying on an unpinned `latest` tag because it makes deployments more predictable.

---

### 16.3 Kubernetes Secrets

Sensitive information such as:

- Passwords
- API keys
- Database credentials
- Tokens

should not be placed directly inside ordinary Kubernetes configuration files.

Kubernetes provides Secret resources for managing sensitive configuration.

Secrets were not required for the Nginx practice in this step.

---

### 16.4 Least Privilege

Production Kubernetes workloads should run with only the permissions they actually require.

Important security controls include:

- RBAC
- Service Accounts
- Network Policies
- Restricted container permissions
- Secure image sources
- Resource limits

These advanced security configurations were outside the scope of this foundational step.

---

### 16.5 Local Cluster Security

Minikube was used only as a local learning environment.

The cluster should not be treated as a production Kubernetes security model.

---

## 17. Verification

The following checks were completed during Step 27.

### 17.1 Kubernetes Cluster

    minikube status

The cluster was successfully running.

The Kubernetes context was:

    minikube

The node was:

    minikube

and its status was:

    Ready

---

### 17.2 Pod Verification

The initial Nginx Pod reached:

    1/1 Running

The Nginx container successfully started.

The container logs confirmed that Nginx was ready to serve requests.

---

### 17.3 Service Verification

The Service was successfully created:

    nginx-service

The Service type was:

    NodePort

The Service exposed:

    80:31270/TCP

The Service had a valid backend Endpoint:

    10.244.0.4:80

---

### 17.4 Application Verification

The Service URL returned:

    HTTP/1.1 200 OK

The response identified:

    Server: nginx/1.31.3

The response body contained the Nginx welcome page.

Therefore:

    Client
       ↓
    NodePort
       ↓
    Service
       ↓
    Pod
       ↓
    Nginx

was successfully verified.

---

### 17.5 Deployment Verification

The Deployment was scaled to three replicas.

Final state:

    DESIRED: 3
    CURRENT: 3
    READY: 3
    AVAILABLE: 3

This confirmed that the Deployment reached its desired state.

---

### 17.6 Self-Healing Verification

One Deployment-managed Pod was manually deleted.

Kubernetes automatically created a replacement Pod.

The final Deployment state returned to:

    3/3 Ready

This verified Kubernetes self-healing behavior through the Deployment and ReplicaSet.

---

### 17.7 Rolling Update Verification

The Nginx image was changed from:

    nginx:alpine

to:

    nginx:1.27-alpine

The Deployment successfully completed the rollout.

The final ReplicaSet state showed:

    Old ReplicaSet
    Desired: 0
    Current: 0
    Ready: 0

    New ReplicaSet
    Desired: 3
    Current: 3
    Ready: 3

The final Pods were running the updated image.

This verified the Rolling Update mechanism.

---

## 18. Key Lessons

### 18.1 Kubernetes Is an Orchestrator

Docker runs containers.

Kubernetes manages containerized workloads across a cluster.

---

### 18.2 Pod Is the Basic Workload Unit

A Pod is the smallest deployable Kubernetes unit.

In this practice:

    nginx-pod
        ↓
    nginx container
        ↓
    Nginx web server

---

### 18.3 Service Provides Stable Network Access

Pod IP addresses can change when Pods are recreated.

A Service provides a stable network endpoint and selects the appropriate Pods using labels.

---

### 18.4 Deployment Manages Desired State

A Deployment allows us to define the desired state of an application.

For example:

    replicas = 3

Kubernetes continuously works toward maintaining that state.

---

### 18.5 ReplicaSet Maintains Pod Count

The ReplicaSet ensures that the required number of Pods exists.

When one Pod was manually deleted, the ReplicaSet created a replacement.

---

### 18.6 Kubernetes Provides Self-Healing

The self-healing test demonstrated:

    Pod deleted
        ↓
    Actual state changed
        ↓
    ReplicaSet detected the difference
        ↓
    Replacement Pod created
        ↓
    Desired state restored

---

### 18.7 Rolling Updates Reduce Disruption

The Nginx image was updated from:

    nginx:alpine

to:

    nginx:1.27-alpine

Kubernetes created new Pods gradually and terminated the old Pods as the new Pods became ready.

This demonstrated the basic idea of controlled application updates.

---

### 18.8 Desired State Is Central to Kubernetes

A core Kubernetes concept learned in this step is:

    Desired State
          ↓
    Kubernetes Controllers
          ↓
    Actual State
          ↓
    Reconciliation

Kubernetes continuously works to make the actual state match the desired state.

---

### 18.9 Troubleshooting Should Follow the Architecture

When something fails, investigate systematically:

    Cluster
      ↓
    Node
      ↓
    Workload
      ↓
    Pod
      ↓
    Container
      ↓
    Service
      ↓
    Endpoint
      ↓
    Application

This is more reliable than randomly running commands.

---

## 19. Final Result

Step 27 — Kubernetes was completed successfully as a Phase 2 supplementary lab.

### Completed Practical Objectives

| Objective | Status |
|---|---|
| Install and verify Kubernetes CLI | Completed |
| Configure local Kubernetes environment | Completed |
| Start Minikube cluster | Completed |
| Verify Kubernetes Node | Completed |
| Create Pod | Completed |
| Run Nginx container inside Pod | Completed |
| Inspect Pod | Completed |
| Read container logs | Completed |
| Use Labels and Selectors | Completed |
| Create NodePort Service | Completed |
| Verify Service Endpoint | Completed |
| Access Nginx through Service | Completed |
| Create Deployment | Completed |
| Create multiple replicas | Completed |
| Scale to 3 replicas | Completed |
| Test self-healing | Completed |
| Perform Rolling Update | Completed |
| Verify updated Pods | Completed |
| Troubleshoot Kubernetes setup issues | Completed |

### Final Kubernetes Workflow

    Containerization
          ↓
       Docker
          ↓
    Kubernetes Cluster
          ↓
        Pod
          ↓
      Deployment
          ↓
      ReplicaSet
          ↓
     Multiple Pods
          ↓
       Service
          ↓
     Application Access

### Final Practice Environment

    Practice:
    /home/afroza/Projects/devops-supplementary-labs/Step-27-Kubernetes

    Documentation:
    /home/afroza/Projects/devops-learning/Step-27-Kubernetes

### Final Status

    Step 27 — Kubernetes
    Phase 2 Supplementary Lab
    Status: COMPLETE

The practical work demonstrated the core Kubernetes workflow:

    Create
      ↓
    Run
      ↓
    Expose
      ↓
    Scale
      ↓
    Recover
      ↓
    Update
      ↓
    Verify

The step established the foundational understanding required before moving to more advanced Kubernetes topics.
