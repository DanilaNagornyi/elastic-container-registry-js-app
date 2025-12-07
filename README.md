# AWS ECR Training - Docker Image Push

> Practical guide for working with Amazon Elastic Container Registry (ECR)

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS_ECR-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)

## 📋 Overview

This is a training application for learning AWS ECR workflows. A simple Node.js application with MongoDB demonstrates the complete cycle:
- Building Docker images
- Authenticating with AWS ECR
- Pushing images to Amazon's private registry

### Tech Stack

- **Frontend**: HTML, CSS, vanilla JavaScript
- **Backend**: Node.js + Express
- **Database**: MongoDB
- **Container Registry**: AWS ECR

---

## 🚀 Quick Start with AWS ECR

### Prerequisites

```bash
# Install AWS CLI (macOS)
brew install awscli

# Verify Docker installation
docker --version
```

### 1. Configure AWS credentials

```bash
aws configure
```

Enter:
- AWS Access Key ID
- AWS Secret Access Key
- Default region: `ap-southeast-2`
- Default output format: `json`

### 2. Create ECR repository

```bash
# Create repository in AWS ECR
aws ecr create-repository --repository-name my-app --region ap-southeast-2
```

### 3. Authenticate with ECR

```bash
aws ecr get-login-password --region ap-southeast-2 | \
  docker login --username AWS --password-stdin \
  678957989472.dkr.ecr.ap-southeast-2.amazonaws.com
```

### 4. Build Docker image

```bash
docker build -t my-app:1.0 .
```

### 5. Tag the image

```bash
docker tag my-app:1.0 \
  678957989472.dkr.ecr.ap-southeast-2.amazonaws.com/my-app:1.0
```

### 6. Push to ECR

```bash
docker push 678957989472.dkr.ecr.ap-southeast-2.amazonaws.com/my-app:1.0
```

### 7. Verify

```bash
# List images in repository
aws ecr list-images --repository-name my-app --region ap-southeast-2
```

---

## 🧪 Local Application Testing

### Option 1: With Docker Compose (recommended)

```bash
# Start MongoDB and Mongo Express
docker-compose up -d

# Create database "user-account" and collection "users" in Mongo Express
# http://localhost:8080

# Start application
cd app
npm install
node server.js

# Open in browser
# http://localhost:3000
```

### Option 2: With individual Docker containers

```bash
# Create Docker network
docker network create mongo-network

# Start MongoDB
docker run -d -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  --name mongodb --net mongo-network mongo

# Start Mongo Express
docker run -d -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
  -e ME_CONFIG_MONGODB_SERVER=mongodb \
  --net mongo-network --name mongo-express mongo-express

# Open Mongo Express: http://localhost:8081
# Create database "user-account" and collection "users"

# Start application
cd app
npm install
node server.js

# Open application: http://localhost:3000
```

---

## 📝 Useful Commands

```bash
# View container logs
docker logs <container-id>

# List running containers
docker ps

# Stop all containers
docker-compose down

# Delete image from ECR
aws ecr batch-delete-image \
  --repository-name my-app \
  --image-ids imageTag=1.0 \
  --region ap-southeast-2
```

---

## 🎯 Learning Objectives

- ✅ Create and manage ECR repositories
- ✅ Authenticate Docker with AWS ECR
- ✅ Tag images for private registries
- ✅ Push and pull images from ECR
- ✅ Work with AWS CLI
