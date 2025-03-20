# <p align="center">Kubernetes (K8s) for Beginners</p>

# Containers
- Containers are lightweight, isolated environments that allow applications to run with their own dependencies, libraries, and settings while sharing the same OS kernel.
- They make it easy to develop, ship, and run applications consistently across different environments.
### Use of Containers
- Solve compatibility issues (no "matrix from hell")
- Run applications consistently across different environments (Dev, Test, Prod)
- Reduce setup time for developers
- Enable faster updates and scaling
### Containers vs. Virtual Machines (VMs)
- Containers share the OS kernel, making them lightweight (MBs in size) and fast to start (seconds).
- VMs include a full OS for each instance, making them larger (GBs in size) and slower to start (minutes).
- VMs provide more isolation, but containers offer better efficiency and resource utilization.
### Use Docker to Run Containers
- **Pulling & Running Containers:** Many applications are already available as **Docker images** in public registries like **Docker Hub**. You can pull and run these containers using `docker run`.
    ```bash
    docker run <image_name>
    ```
    - Each command:
      - Downloads the required image (if not already available).
      - Creates a **container** from the image.
      - Runs the application inside the container.
    - The `-d` flag runs the containers in **detached mode** (in the background).
        ```bash
        docker run <image_name>
        ```
### Docker Image vs. Docker Container
- A **Docker Image** is a **blueprint** or **template** used to create Docker containers. It is **read-only** and contains everything needed to run an application, including code, runtime, dependencies, and configurations. Docker images are built using a **Dockerfile** and are stored in **repositories** like Docker Hub for sharing and reuse.
- A **Docker Container** is a **running instance** of a Docker image. Unlike images, containers are **read-write**, meaning changes can be made while they are running. Containers exist only when they are actively running on a **host machine**. Multiple containers can be created from the same image, allowing for **scalability** and **consistency** across environments. 🚀
### Why Use Containers? (Dev & Ops Benefits)
- **Before Containers (Traditional Deployment):**
  - Developers wrote code and handed it over to Operations.
  - Operations set up environments manually, leading to **inconsistencies**.
  - "Works on my machine" problem.
- **With Containers (Dockerized Deployment):**
  - **Developers** write a `Dockerfile` to define everything needed for the app.
  - **Operations** simply run the image to deploy the app—same across all environments.
  - Reduces **configuration drift** and **dependency issues**.
### Creating Your Own Docker Image
- **Steps:**
  1. **Write a `Dockerfile`:**  
     ```dockerfile
     FROM node:14
     WORKDIR /app
     COPY . .
     RUN npm install
     CMD ["node", "server.js"]
     ```
  2. **Build the Image:**  
     ```bash
     docker build -t myapp .
     ```
  3. **Run a Container from the Image:**  
     ```bash
     docker run -d -p 3000:3000 myapp
     ```
- This ensures the app runs the same way in **Development**, **Testing**, and **Production**.

# Container Orchestration
- Container orchestration is the process of **automating deployment, scaling, networking, and management** of containerized applications. When running applications in **production**, managing multiple containers manually becomes difficult. Orchestration tools handle:
  - **Scaling**: Increase or decrease the number of containers based on traffic.
  - **Load Balancing**: Distribute traffic across multiple containers.
  - **Self-healing**: Restart failed containers automatically.
  - **Networking**: Manage communication between containers.
### Orchestration Technologies
- There are several container orchestration tools available:
  - **Docker Swarm:** Built into Docker, easy to set up, but lacks advanced features.
  - **Kubernetes:** Most popular, highly scalable, supports complex applications.
  - **Apache Mesos:** Powerful but complex, used for large-scale deployments.
- Among these, **Kubernetes** is the most widely used because of its flexibility, cloud provider support, and large community.

# Kubernetes
- Kubernetes is an open-source system for **automating deployment, scaling, and management of containerized applications**.
- It was developed by Google and is now widely used for orchestrating containers across multiple machines.
- Kubernetes (**K8s**) is the leading container orchestration platform. It helps in:
  - **High Availability**: Application remains online even if some containers or nodes fail.
  - **Auto-scaling**: New containers are added automatically based on demand.
  - **Load Balancing**: Ensures traffic is distributed evenly.
  - **Rolling Updates & Rollbacks**: Deploy new versions without downtime.
  - **Resource Management**: Efficient use of CPU & memory across nodes.
### Kubernetes Architecture
#### Nodes (Worker Nodes)
- A **Node** is a **physical or virtual machine** where Kubernetes is installed. It is responsible for **running application workloads** inside containers.
- Kubernetes launches **containers** on these worker nodes.
- Previously, worker nodes were called **"Minions"**.
- If a **node fails**, the application running on it will **go down**. Use **multiple nodes** in a **cluster** to ensure **high availability**.
#### Cluster
- A **Cluster** is a group of **multiple nodes** that work together to run applications reliably.  
- If one node **fails**, another node takes over.
- **Load sharing** is done across multiple nodes to improve performance.  
#### Master Node (Control Plane)
- The **Master Node** is responsible for **managing the cluster**. It ensures that applications run efficiently across nodes.
- The master node:
  - **Monitors worker nodes**
  - **Manages workloads** and **schedules containers**
  - **Handles failures** and redistributes workloads
### Kubernetes Components
- When you install Kubernetes, it includes several **key components**:
  - **API Server** – The **frontend** of Kubernetes that interacts with users, CLI tools (`kubectl`), and other components.
  - **etcd (Key-Value Store)** – Stores **cluster state** and ensures **consistency** across all nodes.
  - **Scheduler** – Assigns **workloads (pods)** to available worker nodes based on resource availability.
  - **Controllers** – Monitors the cluster, detects failures, and **ensures self-healing** by restarting failed components.
  - **Container Runtime** – The software that **runs containers**, such as **Docker, containerd, or CRI-O**.
  - **kubelet** – An **agent** running on each worker node that communicates with the control plane and **ensures containers run correctly**.
- **How Kubernetes Works:**
  1. **A user requests a new application deployment using `kubectl`.**
  2. **The API Server** receives the request and updates the **etcd datastore**.
  3. **The Scheduler** assigns the application to an available worker node.
  4. **The Controller** ensures the application is running and reschedules if needed.
  5. **The Kubelet** on the worker node ensures the containers run as expected.
  6. **The application is now accessible!**
### Master vs Worker Nodes in Kubernetes
- In Kubernetes, there are **two types of nodes**:
  1. **Master Node** (Control Plane)
  2. **Worker Node** (Minions)
- Each node type runs different components that allow Kubernetes to function properly.
- **Master Node (Control Plane):**
  - The **Master Node** is responsible for **managing the entire cluster**. It handles **orchestration, scheduling, and cluster management**.
  - **Components of the Master Node:**
    - **kube-apiserver:** The **entry point** for all Kubernetes commands (kubectl, UI, API).
    - **etcd:** A **key-value store** that stores the cluster state.
    - **controller-manager:** Manages controllers (ReplicationController, Node Controller, etc.).
    - **scheduler:** Assigns workloads (Pods) to Worker Nodes.
- **Worker Node (Minion):**
  - A **Worker Node** runs application containers inside **Pods**.
  - **Components of the Worker Node:**
    - **kubelet:** Talks to the **Master Node**, ensures pods are running.
    - **Container Runtime:** Runs containers (Docker, containerd, CRI-O).
    - **kube-proxy:** Manages networking & load balancing within the cluster.
### kubectl (Kube Control)
- `kubectl` is the **command-line tool** for interacting with a Kubernetes cluster.
- **Basic kubectl Commands:**
  - `kubectl run hello-minikube` → Deploy an application.
  - `kubectl cluster-info` → Get cluster details.
  - `kubectl get nodes` → List all nodes in the cluster.
- **kubectl** is the tool used to **manage** and **deploy applications** in Kubernetes.

# Evolution of Kubernetes and Container Runtimes
- **Early Days of Kubernetes with Docker:**
  - In the beginning, **Docker** was the only container technology available, making it simple and widely adopted.
  - Kubernetes (K8s) was introduced as a container orchestration tool specifically for Docker containers.
  - At that time, Kubernetes **only supported Docker** and did not work with other container runtimes.
- **Introduction of the Container Runtime Interface (CRI):**
  - As Kubernetes became the most popular container orchestration tool, new container runtimes emerged.
  - To allow Kubernetes to support multiple container runtimes, **the Container Runtime Interface (CRI) was introduced**.
  - CRI allows Kubernetes to run **any container runtime** as long as it follows **Open Container Initiative (OCI) standards**.
- **Open Container Initiative (OCI) Standards:**
  - To standardize container runtimes and images, the **Open Container Initiative (OCI)** defined two key specifications:
    - **Image Specification**: Defines how container images should be built and stored.
    - **Runtime Specification**: Defines how container runtimes should execute containers.
  - Any container runtime that follows these OCI standards can integrate with Kubernetes through the **CRI**.
- **DockerShim: Temporary Support for Docker:**
  - Since **Docker was created before CRI**, it did not originally support the CRI standard.
  - However, Docker was the most widely used container runtime, so Kubernetes had to continue supporting it.
  - To solve this, Kubernetes introduced **Dockershim**, a component that allowed Kubernetes to communicate with Docker **without needing CRI**.
  - While other container runtimes worked directly through CRI, Docker continued to function **outside of CRI** using Dockershim.
- **The Role of containerd in Kubernetes:**
  - Docker was more than just a container runtime; it included features like **volume management, authentication, security, and networking**.
  - However, the actual container runtime inside Docker was **`runC`**, which was managed by **`containerd`**.
  - Since **containerd is CRI-compatible**, it can work **directly** with Kubernetes **without requiring Docker**. This allowed Kubernetes to **adopt containerd as its runtime**, eliminating the need for Docker.
- **Deprecation and Removal of Dockershim:**
  - Over time, maintaining Dockershim became unnecessary and added complexity.
  - To simplify Kubernetes, the **Dockershim component was removed in Kubernetes v1.24**.
  - This meant that Kubernetes would **no longer support Docker directly** as a container runtime.
- **Docker Images Still Work with Kubernetes:**
  - Even though Dockershim was removed, **Docker images are still compatible with Kubernetes**.
  - This is because Docker images follow **OCI’s Image Specification**, making them usable with **containerd** and other OCI-compliant runtimes like **CRI-O**.
Here’s your content formatted into **clear paragraph points** for your notes:  
### Containerd, CLI Tools, and CRI in Kubernetes
#### Containerd as a Separate Project
- Although **containerd** was originally a part of Docker, it has now become an **independent project** and is a member of the **Cloud Native Computing Foundation (CNCF)**.
- This means you can install **containerd separately**, without needing to install Docker.
- If you don’t require Docker’s additional features (such as volume management, authentication, or networking), you can use **containerd alone** as your container runtime.
#### CTR: The Debugging Tool for Containerd
- When you install containerd, it includes a command-line tool called **`ctr`**. However, **`ctr` is not designed for production use**.
- Instead, it is mainly used for **debugging containerd**. Since `ctr` is not user-friendly, it is not recommended for managing containers in a production environment.
#### Nerdctl: A Docker-Like CLI for Containerd
- A **better alternative** to `ctr` is **`nerdctl`**, which is also called **Node Control Tool**. It is a command-line tool that provides a user experience **similar to Docker CLI**.
- With `nerdctl`, you can manage containers using familiar commands, making it easier for users who are already familiar with Docker.  
#### Container Runtime Interface (CRI)
- **CRI (Container Runtime Interface)** is a single standardized interface that allows Kubernetes to connect with **CRI-compatible container runtimes**, such as **containerd, CRI-O, and Rocket**.
- This interface enables Kubernetes to work with different container runtimes seamlessly.
#### Crictl: CLI Tool for CRI-Compatible Runtimes
- The Kubernetes community has developed **`crictl`** (CRI Control) as a **command-line tool to interact with CRI-compatible container runtimes**.
- Unlike `nerdctl`, which is specific to containerd, `crictl` works **across multiple container runtimes** that support CRI.
- **It must be installed separately** and is widely used for debugging and interacting with Kubernetes container runtimes.  
#### Updated CRI Endpoints in Kubernetes v1.24
- In Kubernetes **v1.24**, the **default CRI endpoints changed**.
- As a result, users need to manually set the **CRI endpoint** in `crictl` to ensure proper communication with the container runtime.
- This update was introduced to improve flexibility and compatibility across different Kubernetes environments.

# Setup Kubernetes using Minikube
- [Refer Here](https://kubernetes.io/docs/tasks/tools/) for the Official docs.
- We install `minikube` on Ubuntu Linux Instance.
  - **The Resource Requirements:**
    - Instance Type → `t3.medium`
    - Storage Volume → `20 GB`
  - **Security Group Required Ports:**
    - `22` → SSH access to the instance
    - `2379-2380` → etcd communication
    - `6443` → API server (required to communicate with kubectl)
    - `10250` → Kubelet API
    - `10251` → Kube-scheduler
    - `10252` → Kube-controller-manager
    - `30000-32767` → NodePort services (for exposing applications)
### First Install `kubectl`
  - [Refer Here](https://kubernetes.io/docs/tasks/tools/#kubectl) for the Official docs.
  - Install `kubectl` using Official docs and Verify the Installation
    ```sh
    kubectl version --client
    ```
  ![preview](./Images/Kubernetes1.png)
### Install `minikube`
- [Refer Here](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download) for the Official docs.
- **First Install `Container or virtual machine manager`:**
  - Install `Docker`
- **Then Install Minikube:**
  - Install `minikube` using Official docs and Start `minikube`
    ```sh
    minikube start
    ```
  - Verify the Installation
    ```sh
    minikube status
    ```
  ![preview](./Images/Kubernetes2.png)
  - 

# Pods
- [Refer Here](https://kubernetes.io/docs/concepts/workloads/pods/) for the Official docs.  
- A **POD** is the smallest unit in Kubernetes that runs a **containerized application**.
- Kubernetes does **not** run containers directly on worker nodes. Instead, it runs **PODs**, which contain the containers.
- Each **POD** usually runs **one container**, but it can run multiple if needed.
### Scaling Applications with PODs
- If more users start using your application, you need to **increase** the number of running instances.
- To **scale up**, you **create new PODs** instead of adding more containers to an existing POD.
- If a single node is full, Kubernetes can **deploy new PODs on another node** in the cluster.
- To **scale down**, you simply **delete unnecessary PODs**.
- Kubernetes does **not** add more containers to an existing Pod for scaling; instead, it creates additional Pods with the same application instance.
### Multi-Container Pods
- A Pod **can** contain multiple containers, but typically **only when needed**.
- Common use cases:
  - A **main application container** (e.g., web server).
  - A **helper container** for supporting tasks (e.g., logging, file processing).
- **Benefits of Multi-Container Pods:**
  - Containers inside a Pod **share storage** (volumes).
  - Containers inside a Pod **share the same network namespace** (communicate via `localhost`).
  - If the Pod is deleted, all containers inside it are also removed.
### Pods vs. Standalone Docker Containers
- **Standalone Docker:**
  - Requires manually linking helper containers to the main application.
  - Must explicitly define networks and shared volumes.
  - Containers must be manually restarted and mapped to each other.
- **Kubernetes Pods:**
  - Handles networking and storage sharing **automatically**.
  - All containers in a Pod are created, started, and terminated **together**.
  - **More scalable and manageable** for production workloads.
### Deploying a Pod with `kubectl`
- **To create a POD running an Nginx container:**
  ```sh
  kubectl run nginx --image=nginx
  ```  
    - `kubectl run` → Creates a POD.  
    - `--image=nginx` → Uses the Nginx image from Docker Hub.  
- **To check if the POD is running:**
  ```sh
  kubectl get pods
  ```  
    - This will show the status of the POD, such as:
      - `ContainerCreating` → The container is being set up.
      - `Running` → The container is successfully running.
- **Commands:**
  ```sh
  kubectl describe pod <pod_name>
  # Shows more details about the Pod.
  kubectl get pods -o wide
  # Shows additional details of the Pods.
  kubectl delete pod <pod_name>
  # To remove a Pod.
  ```
- By default, a newly created Pod **is not accessible** externally. We need **Services** to expose the Pod to users.