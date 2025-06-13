# 🔐 Connect to EC2 Instance via SSH using AWS CLI

This guide shows how to connect to your Ubuntu EC2 instance via SSH using AWS CLI and your `.pem` key pair.

---

## ✅ Step 1: Get the EC2 Public IP

Run the following command to retrieve the public IP address of your running EC2 instance:

```bash
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query "Reservations[*].Instances[*].PublicIpAddress" \
  --output text
```

This will return something like:

```bash
13.235.117.105
```

---

## 🔒 Step 2: Set Permissions for the Key File

Make sure your key pair file has the proper permissions. Replace the filename as needed:

```bash
chmod 400 eks-keypair.pem
```

---

## 🔗 Step 3: SSH into Your EC2 Instance

Use the following command to connect to your instance:

```bash
ssh -i "eks-keypair.pem" ubuntu@13.235.117.105
```

> 🔁 Replace `eks-keypair.pem` with your actual key file name.
>
> 🌐 Replace `13.235.117.105` with the actual public IP address you obtained in Step 1.

---

## 📌 Notes

* Ensure your Security Group allows inbound traffic on port 22 (SSH).
* Default user for Ubuntu AMI is `ubuntu`.

---

Happy SSH-ing! 🎉
