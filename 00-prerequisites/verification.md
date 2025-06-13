# ✅ Prerequisite Verifications

This document outlines the necessary prerequisites before starting the EKS deployment project. Each section verifies one requirement. If something is missing, the solution to create or fix it is also provided.

---

## 1. ✅ AWS CLI Installed

**Check:**

```bash
aws --version
```

**Expected Output:**

```
aws-cli/2.x.x Python/... Linux/... exe/x86_64.ubuntu...
```

**If not installed:**

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

---

## 2. ✅ AWS CLI Configured

**Check:**

```bash
aws configure
```

> Enter AWS Access Key ID, Secret Access Key, Region (`ap-south-1`), and output format (`json`)

**Verify:**

```bash
aws sts get-caller-identity
```

**Expected Output:**

```
{
  "UserId": "...",
  "Account": "...",
  "Arn": "arn:aws:iam::ACCOUNT_ID:user/USERNAME"
}
```

---

## 3. ✅ IAM User Access

**Check:** Confirm current user has permissions for:

* IAM
* EC2
* EKS

**Solution:** Attach `AdministratorAccess` or appropriate managed policies to the user.

---

## 4. ✅ EC2 Key Pair Exists

**Check:**

```bash
aws ec2 describe-key-pairs
```

**If not found:**

```bash
aws ec2 create-key-pair --key-name eks-keypair --query "KeyMaterial" --output text > eks-keypair.pem
chmod 400 eks-keypair.pem
```

---

## 5. ✅ Security Group Available

**Check:**

```bash
aws ec2 describe-security-groups
```

Look for a group with ports 22 (SSH) and 80 (HTTP) open.

**If not found:**
Create a new one manually via AWS Console or using CLI.

---

## 6. ✅ Subnet & VPC Info

**Check:**

```bash
aws ec2 describe-subnets --query 'Subnets[*].{ID:SubnetId,AZ:AvailabilityZone}'
```

Make sure at least one subnet is available in your target region (e.g. `ap-south-1a`).

**If not found:** Use the AWS Console to create a new VPC with public subnet or use default VPC.

---

✅ Once all the above verifications are complete, you can proceed with the EC2 creation and EKS setup steps.
