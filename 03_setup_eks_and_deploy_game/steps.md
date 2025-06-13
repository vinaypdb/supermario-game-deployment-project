# 🎮 Deploy Super Mario Game on EKS Using Terraform

This guide walks you through the complete setup of an EKS cluster on AWS and deploying the Super Mario game using Terraform and Kubernetes.

---

## 📦 Step 1: Install Required Tools on EC2

Update and install the following tools:

```bash
# Update packages
sudo apt update && sudo apt upgrade -y

# Install Terraform
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform -y

# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

---

## 🔧 Step 2: Configure AWS CLI

```bash
aws configure
# Provide:
# - AWS Access Key ID
# - AWS Secret Access Key
# - Region: ap-south-1
# - Output format: json
```

---

## 📅 Step 3: Clone the Project Repository

```bash
git clone https://github.com/simplilearn10/supermario-game.git
cd supermario-game/EKS-TF
```

---

## 🗂️ Step 4: (Optional) Disable Terraform Backend Configuration

If you don't want to use remote state with S3:

```bash
mv backend.tf backend.tf-excluding
terraform init -reconfigure
```

---

## 🌍 Step 5: Update Provider Region

Edit the `provider.tf` file to match your AWS region:

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

---

## 🚀 Step 6: Deploy Infrastructure Using Terraform

```bash
terraform init
terraform apply --auto-approve
```

---

## 🔗 Step 7: Configure `kubectl` to Connect to EKS

```bash
aws eks update-kubeconfig --region ap-south-1 --name EKS_CLOUD
kubectl get nodes
```

> 🧪 Verify node connectivity with `kubectl get nodes`.

---

## 🎮 Step 8: Deploy Super Mario Game

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## 🌐 Step 9: Access the Game in Your Browser

```bash
kubectl get svc
```

> 🔍 Copy the `EXTERNAL-IP` of `mario-service` and open it in your web browser to play the game.

---

## ✅ Final Output

* ✅ **EKS Cluster is up and running**
* ✅ **Game pods are deployed successfully**
* ✅ **Service is exposed via LoadBalancer**
* ✅ **Game is accessible through a public URL**

---

## 🏊 Project Completed

Enjoy your Super Mario game hosted on a Kubernetes-managed AWS infrastructure! 🎉
