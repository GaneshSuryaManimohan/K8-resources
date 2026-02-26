# Installing kubectl (Linux – amd64)

* This section explains how to manually download and install kubectl for Amazon EKS.

* kubectl is the official Kubernetes CLI used to interact with a Kubernetes cluster.

---

## Step 1: Download the Binary

`curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.35.0/2026-01-29/bin/linux/amd64/kubectl`

* **Explanation:**

1. curl → Downloads files from the internet

2. -O → Saves file with original name

3. The URL points to:
    - Kubernetes version: 1.35.0
    - OS: linux
    - Architecture: amd64
    - Official Amazon EKS S3 distribution bucket

4. This downloads the kubectl binary into your current directory.

---

## Step 2: Make the Binary Executable

`chmod +x ./kubectl`

* **Explanation:**

1. `chmod +x` → Adds execute permission

2. Required because downloaded files are not executable by default

---

## Step 3: Add to User Local Bin Directory

`mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH`

* **Breakdown:**

1. `mkdir -p $HOME/bin`
    - Creates a personal bin directory if it doesn’t exist

2. `cp ./kubectl $HOME/bin/kubectl`
    - Copies the binary into your local bin folder

3. `export PATH=$HOME/bin:$PATH`
    - Temporarily updates your PATH so the system can find kubectl

**⚠️ Note:**
This PATH change is temporary. To make it permanent, add the following to `~/.bashrc` or `~/.bash_profile`: `export PATH=$HOME/bin:$PATH`

---

## Step 4: Move Binary System-Wide (Alternative Method)

`sudo mv kubectl /usr/local/bin`

* **Explanation:**

1. Moves kubectl into `/usr/local/bin`

2. Makes it available globally for all users

3. Requires sudo privileges

**⚠️ Important:**

If you already copied it to `$HOME/bin`, this step is optional.
Choose either:

1. User-level install `($HOME/bin)`

2. System-wide install `(/usr/local/bin)`

**Doing both is redundant.**

--- 

## Step 5: Verify Installation

`kubectl version --client`

Example Output: `Client Version: v1.35.0`

If you see the client version, installation was successful.

---

* **What This Setup Achieves:**

1. Downloads a specific Kubernetes version

2. Grants execute permissions

3. Makes kubectl accessible from anywhere in the terminal

4. Verifies successful installation

---

* **Best Practice Recommendation:**

Instead of hardcoding the version in production environments:

- Match kubectl version closely with your EKS cluster version

- Keep it within ±1 minor version of your cluster

Example: If your EKS cluster runs: 1.34

Recommended kubectl: 1.34.x or 1.35.x

---

# Creating and Managing an EKS Cluster with eksctl and kubectl

This section explains the commands used to:

1. Create an EKS cluster
2. Verify worker nodes
3. Create and manage namespaces
4. Delete a namespace

---

#### 1️⃣ Create EKS Cluster

`eksctl create cluster --config-file=eks.yaml` :

* **What this does:**

1. Uses eksctl to create an Amazon EKS cluster

2. Reads configuration from eks.yaml

3. Automatically provisions:
    - VPC (if not specified)
    - Subnets
    - Security Groups
    - Node Groups (EC2 worker nodes)
    - IAM roles
    - EKS control plane

**Important:** Cluster creation may take 10–20 minutes depending on node size and region.

---

#### 2️⃣ Verify Worker Nodes

`kubectl get nodes` :

* **Purpose:**

Lists all worker nodes registered with the cluster.

Example output:

NAME                           STATUS   ROLES    AGE   VERSION
ip-192-168-xx-xx.ec2.internal  Ready    <none>   2m    v1.35.0

* **Key Field:**

    - `Ready` → Node is healthy and ready to accept pods. If nodes are not `Ready`, workloads cannot be scheduled.

* **Working with Namespaces:**

Namespaces logically isolate resources inside a Kubernetes cluster.

---

#### 3️⃣ Create a Namespace

`kubectl create namespace expense`

Output: `namespace/expense created`

* **What this does:**

1. Creates a new namespace named expense

2. Allows logical separation of applications

3. Useful for multi-project or multi-environment setups

Example use case:

1. default → System or testing

2. expense → Expense tracking application

3. dev, qa, prod → Environment separation

---

#### 4️⃣ List All Namespaces

`kubectl get ns` OR `kubectl get namespaces`

Example output:

```ini
NAME              STATUS   AGE
default           Active   9m22s
expense           Active   10s
kube-node-lease   Active   9m21s
kube-public       Active   9m22s
kube-system       Active   9m22s
```

Explanation of Default Namespaces:
| Namespace | Purpose|
|------|----------|
| default | Default namespace for resources |
| kube-system |	Kubernetes system components |
| kube-public |	Public cluster information |
| kube-node-lease |	Node heartbeat management |
| expense |	Custom namespace you created |

STATUS: Active means the namespace is available for use.