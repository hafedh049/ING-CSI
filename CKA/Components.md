1. **Control Plane Components** (Master Node)
2. **Node Components** (Worker Nodes)

---

## **1️⃣ Control Plane Components**

The **Control Plane** is responsible for managing the cluster, scheduling workloads, and maintaining the desired state.

### **🛠️ Key Control Plane Components:**

1. **API Server (`kube-apiserver`)**
    
    - Acts as the **entry point** for all Kubernetes commands (`kubectl`, API requests).
    - Validates and processes API requests.
    - Communicates with all other components.
2. **Scheduler (`kube-scheduler`)**
    
    - Assigns Pods to worker nodes based on available resources, policies, and constraints.
3. **Controller Manager (`kube-controller-manager`)**
    
    - Runs various controllers that ensure the cluster is in the desired state.
    - Important controllers:
        - **Node Controller** (handles node failures)
        - **Replication Controller** (ensures the correct number of replicas)
        - **Service Account & Token Controller** (manages service accounts)
4. **Etcd (Key-Value Store)**
    
    - Stores **all cluster data** (state, configurations, secrets).
    - It must be **highly available** (HA) and backed up regularly.
5. **Cloud Controller Manager (`cloud-controller-manager`)** _(only in cloud environments)_
    
    - Manages cloud-specific integrations (e.g., AWS, GCP, Azure).

---

## **2️⃣ Node Components (Worker Nodes)**

Worker nodes **run workloads (Pods/containers)** and communicate with the control plane.

### **🛠️ Key Worker Node Components:**

1. **Kubelet**
    
    - The **agent** running on each node.
    - Registers the node with the cluster.
    - Ensures that the specified containers are running inside Pods.
2. **Kube Proxy**
    
    - Maintains network rules and allows communication between services inside the cluster.
3. **Container Runtime**
    
    - Runs containers in Pods.
    - Supported runtimes:
        - **containerd**
        - **CRI-O**
        - **Docker** (deprecated in Kubernetes 1.20+)
4. **Node OS & Networking**
    
    - Each node has an OS (Linux-based) that supports networking, storage, and security.

---

## **3️⃣ Add-Ons (Optional, But Important)**

1. **CoreDNS** (Manages service discovery and internal DNS resolution).
2. **Ingress Controller** (Handles external traffic to services).
3. **Metrics Server** (Provides resource usage metrics).

---

### **📌 Quick Summary (Cheat Sheet)**

| **Component**          | **Description**                                         |
| ---------------------- | ------------------------------------------------------- |
| **API Server**         | Main entry point for Kubernetes commands & API requests |
| **Scheduler**          | Assigns Pods to nodes                                   |
| **Controller Manager** | Runs controllers (Node, Replication, etc.)              |
| **Etcd**               | Key-value store for cluster data                        |
| **Kubelet**            | Runs on worker nodes to ensure Pods are running         |
| **Kube Proxy**         | Handles networking & service communication              |
| **Container Runtime**  | Runs containers (containerd, CRI-O, Docker)             |
