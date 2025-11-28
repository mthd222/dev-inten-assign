# MEAN CRUD Application — Dockerized Deployment with CI/CD & Nginx Reverse Proxy

This repository contains a fully containerized **MEAN Stack (MongoDB, Express, Angular, Node.js)** CRUD application, along with a complete **DevOps deployment pipeline** using:

- **Docker**
- **Docker Compose**
- **Nginx Reverse Proxy**
- **GitHub Actions (CI/CD)**
- **AWS EC2 Ubuntu VM**

The application allows users to create, update, delete, and search tutorials.  
The entire stack runs using Docker Compose, and GitHub Actions automates build → push → deploy.

---

# 📁 Project Structure

```text
crud-dd-task-mean-app/
│
├── .github/
│   └── workflows/
│       └── ci-cd.yml          # GitHub Actions pipeline configuration
│
├── backend/                   # Node.js + Express API
│   ├── Dockerfile             # Backend container definition
│   ├── package.json           # Backend dependencies
│   ├── server.js              # Entry point for the API
│   └── ...
│
├── frontend/                  # Angular application
│   ├── Dockerfile             # Frontend container definition (Multi-stage)
│   ├── package.json           # Frontend dependencies
│   ├── src/                   # Source code
│   └── ...
│
├── nginx/                     # Reverse Proxy Configuration
│   └── default.conf           # Nginx routing rules
│
├── docker-compose.yml         # Orchestrates Backend, Frontend, and Database
└── README.md                  # Project documentation
```
# Docker Architecture

### Services used:
| Service     | Description |
|-------------|-------------|
| **mongo**   | MongoDB database (official image) |
| **backend** | Express API (Node.js) |
| **frontend** | Angular UI served via nginx |
| **nginx** | Reverse proxy routing UI + API |

### 📌 Backend reachable only via Nginx  
Angular **never directly talks to port 3000**. 
Instead, it calls:
/api/tutorials

Nginx proxies this to the backend.

# Docker Compose (Multi-Container Deployment)
The `docker-compose.yml` spins up:
- MongoDB 
- Backend REST API 
- Angular frontend 
- Nginx reverse proxy (serves frontend + backend) 

To run locally:
docker compose up -d --build

Access website:
http://localhost/

CI/CD Pipeline (GitHub Actions)

This project includes a full CI/CD pipeline at:
.github/workflows/ci-cd.yml
Pipeline Steps:
Checkout code
Build Docker images
frontend → YOUR_DOCKERHUB_USER/mean-frontend:latest
backend → YOUR_DOCKERHUB_USER/mean-backend:latest
Push images to Docker Hub
VM HOST public ip address
SSH into AWS VM
Pull latest images
Restart containers via Docker Compose
On every push to main branch

AWS Deployment Guide
1. Launch EC2 Server
Ubuntu 22.04
Open ports:
22 (SSH)
80 (HTTP)

2. Install Docker & Compose
sudo apt update -y
sudo apt install -y docker.io docker-compose-plugin
sudo usermod -aG docker ubuntu
newgrp docker

3. Deploy
docker compose up -d --build

4. Visit website
http://YOUR_PUBLIC_IP/
