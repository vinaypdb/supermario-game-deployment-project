# 🧱 Option 1: Create EC2 Instance and Associate IAM Role via AWS Console

This step sets up the base infrastructure for deploying workloads to AWS. You'll create an IAM role with appropriate permissions and then launch an EC2 instance with that role attached.

---

## 🔐 Step 1: Create IAM Role for EC2

### 🎯 Why this step?
To allow the EC2 instance to interact with AWS services like EKS, S3, and EC2, we need to create and attach an IAM role with the necessary permissions.

### 📝 Instructions

1. Navigate to **IAM > Roles** in the AWS Console.
2. Click **Create role**.
3. Configure the following:
   - **Trusted entity type**: `AWS service`
   - **Use case**: `EC2`
4. Click **Next** and attach the following policies:
   - `AmazonEC2FullAccess`
   - `AmazonS3FullAccess`
   - `AmazonEKSClusterPolicy`
   - `AmazonEKSWorkerNodePolicy`
   - `AmazonEKS_CNI_Policy`
   - `AmazonEKSServicePolicy`
5. Name the role: `EC2AdminEKSRole`
6. Click **Create role**.

---

## 🖥️ Step 2: Launch an EC2 Instance

### 🎯 Why this step?
This instance will act as your control plane to set up and manage the EKS cluster using tools like Terraform, kubectl, and AWS CLI.

### 📝 Instructions

1. Go to **EC2 > Launch Instance**.
2. Select **Ubuntu Server 22.04 LTS (x86)** as the AMI.
3. Choose `t2.micro` as the instance type (Free Tier eligible).
4. Under **Advanced Details**, assign the IAM role: `EC2AdminEKSRole`.
5. Configure a **Security Group** with the following inbound rules:
   - **SSH (Port 22)** – for remote access
   - **HTTP (Port 80)** – for accessing web apps
6. Launch the instance and download the key pair if needed.
7. Connect to the instance via SSH once it’s running.

---

✅ **Next Step:** [Connect to EC2 via SSH →](../02_Connect_EC2_through_SSH/steps.md)
