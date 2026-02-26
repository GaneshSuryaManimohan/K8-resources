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