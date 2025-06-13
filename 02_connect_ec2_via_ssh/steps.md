# 🔐 Connect to EC2 Instance via SSH (AWS CLI)

This guide walks you through securely connecting to your Ubuntu EC2 instance using SSH, AWS CLI, and your `.pem` key pair.

---

## ✅ Step 1: Retrieve Public IP of EC2 Instance

Use the following AWS CLI command to get the public IP of your running instance:

```bash
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query "Reservations[*].Instances[*].PublicIpAddress" \
  --output text
```

📟 **Example Output:**

```bash
13.235.117.105
```

---

## 🔒 Step 2: Set Key File Permissions

Ensure your SSH key file has secure permissions (required for SSH to work):

```bash
chmod 400 eks-keypair.pem
```

> Replace `eks-keypair.pem` with the actual filename of your key pair.

---

## 🔗 Step 3: Connect to EC2 via SSH

Now connect to the instance using:

```bash
ssh -i "eks-keypair.pem" ubuntu@13.235.117.105
```

> ✅ Replace:
>
> * `eks-keypair.pem` with your private key filename
> * `13.235.117.105` with your EC2 instance’s public IP

---

## ℹ️ Additional Notes

* ✅ **Security Group:** Ensure it allows inbound traffic on port **22 (SSH)**.
* 🧑‍💻 **Default Username:** For Ubuntu AMIs, the default SSH user is `ubuntu`.

---

🎉 You are now connected to your EC2 instance!
