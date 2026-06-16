# 💓 Heart Rate Monitor App — Docker Multistage Build + Push to Elastic Container Registry (ECR)

---

## 🏢 Industry Scenario

A DevOps Engineer is working at a **healthcare tech company** that delivers lightweight, secure, and optimized container images for a **Heart Rate Monitoring application**. To minimize Docker image size in the CI/CD pipeline and production environment, the engineer uses a **Docker Multistage Build**.

This technique removes unnecessary build dependencies from the final image — critical for speed, security, and reliability in modern health-tech microservices architectures.

---

## 🎯 Objective

- Clone and containerize the Heart Rate Monitor app using a **Docker Multistage Build**
- Build and push the image to **AWS ECR (Elastic Container Registry)**
- Run the app on an **AWS EC2 instance**, expose port **8000**
- Use proper **Security Group** configuration
- Ensure the app runs successfully on a **public IP**

---

## ✅ Step-by-Step Implementation

### 📋 Requirements

| Component       | Details                                               |
|----------------|-------------------------------------------------------|
| Cloud Provider  | AWS EC2 (Ubuntu 22.04 LTS)                           |
| Tools           | Docker, Git, Python, AWS CLI                         |
| App Repository  | https://github.com/nasirbloch323/Heart-RateMonitor-ECR.git |
| Container Port  | 8000                                                  |
| ECR             | AWS Elastic Container Registry (Private Repository)  |

---

## 🚀 Step 0: Launch and Connect to EC2 Instance

From AWS Console:
- Launch EC2 with **Ubuntu 22.04**
- Add **Security Group Inbound Rules**:
  - SSH (22) from My IP
  - Custom TCP (**8000**) from `0.0.0.0/0`

SSH into EC2:
```bash
ssh -i your-key.pem ubuntu@<EC2_PUBLIC_IP>
```

---

## 🐳 What is a Docker Multi-Stage Build?

A multi-stage build allows you to use **multiple FROM statements** in one Dockerfile to create more efficient and secure images.

- ✅ Reduce image size by excluding build dependencies
- ✅ Improve performance and security
- ✅ Separate build and runtime environments

### 🧱 Basic Idea

| Stage          | Purpose                                                    |
|----------------|------------------------------------------------------------|
| **Builder**    | Install dependencies, compile code, set up environment     |
| **Final Image**| Only runtime files copied here — clean, small, production-ready |

> 🧠 Think of it like:
> - **Builder image** = Kitchen (preparing ingredients)
> - **Final image** = Plate served to the customer (no knives or cutting boards)

| Benefit           | Explanation                                      |
|-------------------|--------------------------------------------------|
| ✅ Smaller images  | Keeps only what's necessary to run your app      |
| ✅ Faster deploys  | Smaller images = quicker transfer and startup    |
| ✅ Better security | Fewer tools = smaller attack surface             |
| ✅ Cleaner design  | Separation of build vs run responsibilities      |

---

## 📁 Step 1: Clone the GitHub Repository

SSH into EC2 and install dependencies:
```bash
sudo apt update && sudo apt install -y git docker.io
sudo usermod -aG docker ubuntu
newgrp docker
```

Clone the Heart Rate Monitor project:
```bash
git clone https://github.com/nasirbloch323/Heart-RateMonitor-ECR.git
cd Heart-RateMonitor-ECR
```

---

## 🐳 Step 2: Create a Multistage Dockerfile

Create a file named `Dockerfile` in the project root:

```dockerfile
# 👷 Stage 1: Builder — Install dependencies
FROM python:3.10-slim as builder

WORKDIR /app
# Copy only requirements to leverage Docker cache
COPY requirements.txt .

RUN pip install --upgrade pip \
    && pip install --prefix=/install -r requirements.txt
# Install all dependencies into /install (isolated from final image)

# 📦 Stage 2: Final Image — Clean and production-ready
FROM python:3.10-slim

WORKDIR /app

# Disable output buffering for real-time logs
ENV PYTHONUNBUFFERED=1

# Copy installed packages from builder into the final image
COPY --from=builder /install /usr/local

# Copy all application source code
COPY . .

# Expose the Heart Rate Monitor app port
EXPOSE 8000

# Start the application
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

> ✅ This Dockerfile installs dependencies in a **builder stage**, then copies only what's needed to the **final image** — keeping it lightweight and secure.

---

## 🏗 Step 3: Build Docker Image

```bash
docker build -t heart-rate-monitor-ecr .
```

✅ Sample Output:
```
Successfully built a1b2c3d4e5f6
Successfully tagged heart-rate-monitor-ecr:latest
```

---

## ☁️ Step 4: Push Image to AWS ECR

### 🛠 Install AWS CLI

```bash
# 1. Update packages
sudo apt update

# 2. Install dependencies
sudo apt install unzip curl -y

# 3. Download AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

# 4. Unzip
unzip awscliv2.zip

# 5. Install
sudo ./aws/install

# 6. Verify
aws --version
```

---

### 🧾 Get AWS Credentials

You'll need:
- **Access Key ID**
- **Secret Access Key**

To generate them:
1. Go to **AWS Console**
2. Open **IAM → Users → Your Username**
3. Click **Security credentials**
4. Click **Create access key**
5. Copy both keys

---

### ⚙️ Configure AWS CLI

```bash
aws configure
```

Enter your details:
```
AWS Access Key ID     : AKIAxxxxxxxxxxxxxxxx
AWS Secret Access Key : wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name   : us-east-1
Default output format : json
```

---

### 4.1 Create ECR Repository

```bash
aws ecr create-repository --repository-name heart-rate-monitor-ecr
```

---

### 4.2 Authenticate Docker to ECR

```bash
aws ecr get-login-password --region <your-region> | docker login \
  --username AWS \
  --password-stdin \
  <aws_account_id>.dkr.ecr.<your-region>.amazonaws.com
```

---

### 4.3 Tag the Image for ECR

```bash
docker tag heart-rate-monitor-ecr:latest \
  <aws_account_id>.dkr.ecr.<your-region>.amazonaws.com/heart-rate-monitor-ecr:latest
```

---

### 4.4 Push to ECR

```bash
docker push \
  <aws_account_id>.dkr.ecr.<your-region>.amazonaws.com/heart-rate-monitor-ecr:latest
```

---

### 4.5 Pull from ECR (optional)

```bash
docker pull \
  <aws_account_id>.dkr.ecr.<your-region>.amazonaws.com/heart-rate-monitor-ecr:latest
```

---

## 🐳 Step 5: Run Docker Container on Port 8000

```bash
docker run -d \
  --name heart-rate-monitor \
  -p 8000:8000 \
  <aws_account_id>.dkr.ecr.<your-region>.amazonaws.com/heart-rate-monitor-ecr:latest
```

✅ Output:
```
Container started successfully on port 8000
```

---

## 🔓 Step 6: Add Inbound Rule for Port 8000

Go to:
**AWS Console → EC2 → Security Groups → Inbound Rules**

Add Rule:
- **Type**: Custom TCP
- **Port Range**: 8000
- **Source**: `0.0.0.0/0`

---

## 🌐 Step 7: Access the Heart Rate Monitor App

Open your browser and visit:

```
http://<EC2_PUBLIC_IP>:8000
```

You should see the **Heart Rate Monitor App** running live! 💓

---

## ✅ Conclusion

This project demonstrates a real-world **DevOps deployment** of a Heart Rate Monitor application using **Docker Multistage Builds** for optimized and secure containers. You learned:

- ✅ What Docker multistage builds are and why they matter
- ✅ How to structure a multistage Dockerfile for a Python app
- ✅ How to push a Docker image to **AWS ECR**
- ✅ How to deploy on **AWS EC2** and expose it publicly
- ✅ How to configure Security Groups for port 8000

---

## 📬 Author

**Nasir Bloch**
- 🐙 GitHub: [@nasirbloch323](https://github.com/nasirbloch323)
- 💼 LinkedIn: [linkedin.com/in/nasirbloch323](https://www.linkedin.com/in/nasirbloch323)

> ⭐ If this project helped you, give it a star on GitHub!
