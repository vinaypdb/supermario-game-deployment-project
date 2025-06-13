### 1. **Install Required Tools**

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
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

### 2. **Configure AWS CLI**

```bash
aws configure
# Enter Access Key, Secret, Region (ap-south-1)
```

### 3. **Clone the Project Repository**

```bash
git clone https://github.com/simplilearn10/supermario-game.git
cd supermario-game/EKS-TF
```

### 4. **Update Terraform Backend (optional)**

If you don't want to use `backend.tf` with S3:

```bash
mv backend.tf backend.tf-excluding
terraform init -reconfigure
```

### 5. \*\*Update Region in \*\***`provider.tf`**

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

### 6. **Run Terraform Commands**

```bash
terraform init
terraform apply --auto-approve
```

### 7. **Configure ****`kubectl`**** with EKS**

```bash
aws eks update-kubeconfig --region ap-south-1 --name EKS_CLOUD
kubectl get nodes
```

### 8. **Deploy Super Mario Game to EKS**

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### 9. **Access the Game**

```bash
kubectl get svc
# Note the EXTERNAL-IP for mario-service and open it in a browser
```

---

## ✅ Final Output

* **EKS Cluster** with nodes ready
* **Pods running** for game deployment
* **Service exposed** via LoadBalancer
* **Playable game in browser** using public LoadBalancer URL

---

## 🏁 Project Completed!

---
