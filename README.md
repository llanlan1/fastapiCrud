# FastAPI CRUD API Deployment on EC2 with Docker

This guide documents the steps to deploy a FastAPI CRUD API on an EC2 instance using Docker, including troubleshooting issues encountered along the way.

## Prerequisites
- AWS account with access to EC2
- Docker installed on your local machine
- Basic understanding of FastAPI
- SSH access to your EC2 instance
- Python installed locally for development

## Step 1: Set Up Project Structure
Create a new directory for your FastAPI project:
```sh
mkdir fastapi-crud
cd fastapi-crud
```
Inside this directory, create the following structure:
```
fastapi-crud/
│-- app/
│   │-- main.py  # Entry point of the FastAPI app
│   │-- models.py  # Database models
│   │-- database.py  # Database connection
│   │-- schemas.py  # Pydantic models for request validation
│   │-- crud.py  # CRUD functions
│   │-- routers/
│   │   │-- items.py  # API routes for handling CRUD
│-- requirements.txt  # Dependencies
│-- Dockerfile  # Docker configuration
│-- .env  # Environment variables
│-- README.md  # Documentation
```
The file contents can be copied from this repo.

## Step 2: Set Up an EC2 Instance
1. Log in to your AWS console and navigate to EC2.
2. Launch a new instance using an Amazon Linux 2 or Ubuntu AMI.
3. Select an instance type (e.g., `t2.micro` for free tier users).
4. Configure security group:
   - Allow SSH (port 22) from your IP.
   - Allow HTTP (port 80) and/or HTTPS (port 443).
   - Allow your FastAPI app port (e.g., 8000).
5. Create or use an existing key pair for SSH access.
6. Launch the instance and note its public IP address.

## Step 3: Connect to Your EC2 Instance
```sh
ssh -i your-key.pem ec2-user@your-ec2-ip
```
For Ubuntu:
```sh
ssh -i your-key.pem ubuntu@your-ec2-ip
```

## Step 4: Install Docker on EC2
```sh
sudo yum update -y  # For Amazon Linux
sudo apt update -y  # For Ubuntu
sudo yum install -y docker  # Amazon Linux
sudo apt install -y docker.io  # Ubuntu
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user  # Add user to Docker group
```
Logout and log back in to apply group changes.

## Step 5: Copy Your FastAPI Project to EC2
Use SCP (or Git if you have a repo):
```sh
scp -i your-key.pem -r fastapi-crud ec2-user@your-ec2-ip:~
```

## Step 6: Build & Run the Docker Container on EC2
```sh
cd fastapi-crud
docker build -t fastapi-app .
docker run -d -p 8000:8000 --restart always fastapi-app
```

## Step 7: Allow Inbound Traffic on EC2 (Security Group Settings)
1. Go to AWS EC2 Dashboard → Security Groups.
2. Add an **Inbound Rule** for **port 8000** with **Source: 0.0.0.0/0** (or your IP for security).

## Step 8: Access Your FastAPI App
Visit in your browser:
```sh
http://your-ec2-public-ip:8000
```
Visit FastAPI Swagger UI:
```sh
http://your-ec2-public-ip:8000/docs
```

## Troubleshooting Steps
- **Cannot access FastAPI from browser:**
  - Check if the security group allows traffic on port 8000.
  - Run `docker ps` to ensure the container is running.
  - Inspect logs with `docker logs <container_id>`.
- **Docker command not found:**
  - Ensure Docker is installed and running (`sudo systemctl start docker`).
- **Permissions issue with Docker:**
  - Run `sudo usermod -aG docker $USER` and restart your session.

## Conclusion
Your FastAPI CRUD API is now running on an EC2 instance using Docker! 🎉

