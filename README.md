## 1. AWS CLI Installation
# Update system and install prerequisites
sudo apt update && sudo apt install curl unzip -y

# Download and install AWS CLI v2
curl "[https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip)" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Verify installation
aws --version


## 2. Python 3 Setup & Commands

# Install Python 3, Pip, and Virtual Environment
sudo apt update && sudo apt install python3 python3-pip python3-venv -y

# Verify installation
python3 --version
pip3 --version

# Virtual Environment Setup
python3 -m venv venv
source venv/bin/activate

# Dependency Management
pip install -r requirements.txt
pip freeze > requirements.txt

## 3. Docker Installation & Core CLI

cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update && sudo apt install docker-ce docker-ce-cli containerd.io -y

# Run Docker commands without 'sudo'
sudo usermod -aG docker $USER

# Docker CLI Reference
docker build -t image_name:tag .
docker run -d -p 8080:8080 --name container_name image_name:tag
docker ps
docker ps -a
docker logs -f container_name
docker stop container_name && docker rm container_name
docker system prune -a --volumes -y


## 4. AWS ECR Workflow

```bash
# 1. Configure AWS Credentials
aws configure

# 2. Login to AWS ECR Registry
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com

# 3. Create ECR Repository
aws ecr create-repository --repository-name <repo_name> --region <region>

# 4. Tag Local Image for ECR
docker tag <local_image>:latest <aws_account_id>.dkr.ecr.<region>[.amazonaws.com/](https://.amazonaws.com/)<repo_name>:latest

# 5. Push Image to ECR
docker push <aws_account_id>.dkr.ecr.<region>[.amazonaws.com/](https://.amazonaws.com/)<repo_name>:latest

```

---

## 5. Standard Ubuntu CLI Utilities

```bash
pwd                           # Current directory path dekhna
ls -la                        # Hidden files ke sath details list karna
cd ..                         # Ek folder peeche jana
mkdir -p path/to/dir          # Nested directories banana
rm -rf <file_or_dir>          # Force delete file ya folder
htop                          # CPU and RAM usage monitor karna


```

```
