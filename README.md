# 🎮 Super Mario Game Deployment on EKS with Terraform and AWS CLI

This project demonstrates the complete automation of deploying a **Super Mario game** on a highly available **EKS cluster** using **Terraform**, **AWS CLI**, and **kubectl**.

The goal is to provide an **end-to-end infrastructure-as-code (IaC)** solution to launch, configure, and run a production-grade Kubernetes setup on AWS.

---

## 🚀 Tech Stack Used

| Tool             | Purpose                           |
| ---------------- | --------------------------------- |
| AWS EC2          | Compute instance for provisioning |
| IAM              | Secure access using roles         |
| EKS (Kubernetes) | Orchestrating containers          |
| Terraform        | Infrastructure as Code            |
| AWS CLI          | Command-line AWS operations       |
| kubectl          | Interact with EKS cluster         |
| Git & GitHub     | Version control & collaboration   |

---

## 📁 Folder Structure

```bash
supermario-game-deployment-project/
├── 01_Methods_To_Craete_EC2_and_Associate_IAM_Role/
│   ├── Option1_through_AWS_Console.md
│   └── Option2_through_AWS_CLI.md
├── 02_Connect_EC2_through_SSH/
│   └── connect_via_ssh.md
├── 03_Setup_EKS_and_Deploy_Game/
│   └── deploy_supermario_on_eks.md
└── README.md
```

---

## 🔀 Complete Workflow

### ✅ Step 1: Create EC2 Instance & IAM Role

📄 Refer:

* [`Option1_through_AWS_Console.md`](./01_Methods_To_Craete_EC2_and_Associate_IAM_Role/Option1_through_AWS_Console.md)
* [`Option2_through_AWS_CLI.md`](./01_Methods_To_Craete_EC2_and_Associate_IAM_Role/Option2_through_AWS_CLI.md)

> Create a secure IAM Role and launch an EC2 instance with permissions to manage EKS, EC2, and S3.

---

### ✅ Step 2: Connect to EC2 via SSH

📄 Refer:

* [`connect_via_ssh.md`](./02_Connect_EC2_through_SSH/connect_via_ssh.md)

> Use your key pair and public IP to connect to the EC2 instance and prepare the environment.

---

### ✅ Step 3: Setup EKS and Deploy Super Mario Game

📄 Refer:

* [`deploy_supermario_on_eks.md`](./03_Setup_EKS_and_Deploy_Game/deploy_supermario_on_eks.md)

> Install required tools, configure AWS CLI, clone repo, deploy EKS cluster using Terraform, and deploy the Mario game on Kubernetes.

---

## 💽 Final Output

* ✅ EKS Cluster provisioned via Terraform
* ✅ EC2 node joined and visible via `kubectl get nodes`
* ✅ Super Mario app deployed using `kubectl apply`
* ✅ Accessible public LoadBalancer URL to play the game 🎮

---

## 👨‍💻 Author

* **Name**: vinaypdb
* **GitHub**: [github.com/vinaypdb](https://github.com/vinaypdb)

---

## 📢 Contributions

Feel free to fork, improve, and open pull requests to make this walkthrough even better for new learners and DevOps enthusiasts.

---

## 📜 License

This project is open-source under the [MIT License](LICENSE).

---
