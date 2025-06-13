# EC2 Instance Creation with IAM Role using AWS CLI

This guide explains how to create an IAM Role and launch an EC2 instance using the AWS CLI. These instructions are well-documented to ensure reproducibility for others following the project.

---

## 📌 Prerequisites

* AWS account with programmatic access (Access Key ID and Secret Access Key)
* AWS CLI installed and configured (`aws configure`)
* Proper IAM permissions to create roles and EC2 instances

---

## 🛠️ Step-by-Step Instructions

### Step 1: Configure AWS CLI

```bash
aws configure
# Enter your AWS Access Key, Secret Key, region (e.g. ap-south-1), and output format (json)
```

---

### Step 2: Create IAM Role for EC2

1. **Create trust policy** (allows EC2 to assume this role):

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

2. **Create IAM Role**:

```bash
aws iam create-role \
  --role-name EC2AdminEKSRole \
  --assume-role-policy-document file://trust-policy.json
```

3. **Attach policies to the role**:

```bash
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEKSClusterPolicy
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy
aws iam attach-role-policy --role-name EC2AdminEKSRole --policy-arn arn:aws:iam::aws:policy/AmazonEKSServicePolicy
```

4. **Create instance profile and attach role to it**:

```bash
aws iam create-instance-profile --instance-profile-name EC2AdminEKSProfile
aws iam add-role-to-instance-profile \
  --instance-profile-name EC2AdminEKSProfile \
  --role-name EC2AdminEKSRole
```

> 🔄 Wait a few seconds for the instance profile to be fully available.

---

### Step 3: Launch EC2 Instance with IAM Role

```bash
aws ec2 run-instances \
  --image-id ami-xxxxxxxxxxxxxxxxx \  # Use Ubuntu 22.04 AMI ID for your region
  --instance-type t2.micro \
  --iam-instance-profile Name=EC2AdminEKSProfile \
  --key-name your-keypair-name \  # Replace with your existing keypair
  --security-group-ids sg-xxxxxxxxxxxxxxxxx \  # Must allow port 22 (SSH) and 80 (HTTP)
  --subnet-id subnet-xxxxxxxxxxxxxxxxx \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=EKS-Setup-Node}]'
```

---

## ✅ Result

* EC2 instance with IAM role capable of managing EKS, EC2, and S3
* Easily SSH into it and begin deploying infrastructure via Terraform

---

## 📘 Notes

* To get the correct **Ubuntu 22.04 AMI ID**, visit [https://cloud-images.ubuntu.com/locator/](https://cloud-images.ubuntu.com/locator/)
* You can verify the IAM role is associated by describing the instance:

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,IamInstanceProfile.Arn]" --output table
```

---


