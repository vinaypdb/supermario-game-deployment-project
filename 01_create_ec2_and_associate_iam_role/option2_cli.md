# 🧱 Option 2: Create EC2 Instance with IAM Role via AWS CLI

This document provides a fully scripted, reproducible method to create an EC2 instance and associate it with an IAM role using the AWS CLI. This is particularly useful for automation and infrastructure-as-code pipelines.

---

## 📌 Prerequisites

Before you begin, make sure the following requirements are met:

* ✅ AWS CLI installed and configured (`aws configure`)
* ✅ IAM credentials with permissions to manage EC2 and IAM
* ✅ A key pair already created in your AWS region (for SSH access)

---

## 🛠️ Step-by-Step Instructions

### 🔧 Step 1: Configure AWS CLI

**🌟 Why?**
To authenticate and set your default region/profile for all subsequent AWS CLI commands.

```bash
aws configure
# Input your:
# - AWS Access Key
# - AWS Secret Key
# - Region (e.g. ap-south-1)
# - Output format (e.g. json)
```

---

### 🔐 Step 2: Create IAM Role and Instance Profile

#### 2.1 Define Trust Policy

**🌟 Why?**
Allows EC2 instances to assume the role securely.

```bash
cat > trust-policy.json <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF
```

#### 2.2 Create IAM Role

```bash
aws iam create-role \
  --role-name EC2AdminEKSRole \
  --assume-role-policy-document file://trust-policy.json
```

#### 2.3 Attach Required Policies

```bash
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEKSClusterPolicy
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEKSServicePolicy
```

#### 2.4 Create Instance Profile and Attach Role

```bash
aws iam create-instance-profile --instance-profile-name EC2AdminEKSProfile

aws iam add-role-to-instance-profile \
  --instance-profile-name EC2AdminEKSProfile \
  --role-name EC2AdminEKSRole
```

> 🕒 **Note:** Wait 10–20 seconds for the instance profile to be fully available before launching the instance.

---

### 🚀 Step 3: Launch EC2 Instance with IAM Role

**🌟 Why?**
This EC2 instance will act as your base to run Terraform, interact with EKS, and deploy applications.

```bash
aws ec2 run-instances \
  --image-id ami-xxxxxxxxxxxxxxxxx \         # Ubuntu 22.04 AMI ID for your region
  --instance-type t2.micro \
  --iam-instance-profile Name=EC2AdminEKSProfile \
  --key-name your-keypair-name \             # Replace with your actual key pair
  --security-group-ids sg-xxxxxxxxxxxxxxxxx \  # SG should allow port 22 (SSH), 80 (HTTP)
  --subnet-id subnet-xxxxxxxxxxxxxxxxx \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=EKS-Setup-Node}]'
```

---

## ✅ Output

After completion, you will have:

* An EC2 instance running Ubuntu 22.04
* IAM role with access to EKS, EC2, and S3
* SSH-ready instance for further automation or deployment

---

## 📘 Notes

* To find the correct **Ubuntu 22.04 AMI ID**, use:
  [https://cloud-images.ubuntu.com/locator/](https://cloud-images.ubuntu.com/locator/)

* To verify the IAM role was attached correctly:

```bash
aws ec2 describe-instances \
  --query "Reservations[*].Instances[*].[InstanceId,IamInstanceProfile.Arn]" \
  --output table
```

---

## 🛍️ Next Step

👉 [Connect to the EC2 instance via SSH](../02_Connect_EC2_through_SSH/steps.md)
