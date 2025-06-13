
---

## 🔧 Create EC2 Instance and Associate IAM Role

### 1. **Create IAM Role**

* Go to **IAM > Roles** in AWS Console.
* Click **Create role**:

  * **Trusted entity type**: AWS service
  * **Use case**: EC2
  * Click **Next**.
* Attach the following policies:

  * `AmazonEC2FullAccess`
  * `AmazonS3FullAccess`
  * `AmazonEKSClusterPolicy`
  * `AmazonEKSWorkerNodePolicy`
  * `AmazonEKS_CNI_Policy`
  * `AmazonEKSServicePolicy`
* Name the role: `EC2AdminEKSRole`
* Click **Create role**

### 2. **Launch EC2 Instance (Ubuntu 22.04)**

* Go to **EC2 > Launch instance**
* Select **Ubuntu 22.04 AMI**
* Instance type: `t2.micro` (Free Tier)
* Under **Advanced details > IAM instance profile**, select `EC2AdminEKSRole`
* Add a **Security Group** allowing:

  * Port **22** (SSH)
  * Port **80** (HTTP)
* Launch the instance and connect via SSH

---

