# MEAN CRUD App — Containerized Deployment (Docker / Docker Compose / Nginx / GitHub Actions CI-CD)

**Project:** crud-dd-task-mean-app  
**Author:** Milan Tej H D
**Date:** 28-Nov-2025

---

## Overview

This repository contains a containerized MEAN stack (MongoDB / Express / Angular / Node.js) application and full CI/CD pipeline that builds Docker images, pushes them to Docker Hub, and deploys the application to an Ubuntu VM using Docker Compose. Nginx is used as a reverse proxy to expose the application on port **80**.

---

## Repository Structure

├── backend/
│ ├── package.json
│ ├── server.js
│ └── ... (backend source)
├── frontend/
│ ├── package.json
│ ├── src/
│ └── ... (Angular source)
├── nginx/
│ └── default.conf
├── Dockerfile.backend
├── Dockerfile.frontend
├── docker-compose.yml
├── .github/
│ └── workflows/ci-cd.yml
└── README.md



> Note: The repo contains both Dockerfiles, docker-compose, and the GitHub Actions workflow required for CI/CD.

---

## Prerequisites

- GitHub account.
- Docker Hub account (username: `YOUR_DOCKERHUB_USER`).
- AWS EC2 Ubuntu VM (or similar) reachable via SSH.
- GitHub repo secrets configured:
  - `DOCKERHUB_USERNAME`
  - `DOCKERHUB_TOKEN` (Docker Hub access token or password)
  - `VM_HOST` (public IP or DNS of VM)
  - `VM_USER` (deploy user)
  - `VM_SSH_KEY` (private key contents, newline-encoded)
  - `DOCKERHUB_REPO_FRONTEND` (e.g. `youruser/mean-frontend`)
  - `DOCKERHUB_REPO_BACKEND` (e.g. `youruser/mean-backend`)

---

## Files to review / edit

- `nginx/default.conf` — reverse proxy config
- `docker-compose.yml` — compose file for VM deploy
- `frontend/src/app/services/tutorial.service.ts` — ensure `baseUrl = '/api/tutorials'`
- `backend/server.js` — ensure `/health` endpoint exists (snippet below)
- `.github/workflows/ci-cd.yml` — CI/CD pipeline

---

## How to push this repo to GitHub

From project root:

```bash
git init
git add .
git commit -m "Initial commit - MEAN app containerized + CI-CD"
# create repo on GitHub via UI or gh cli:
# gh repo create YOUR_GITHUB_USER/crud-dd-task-mean-app --public --confirm
git remote add origin git@github.com:YOUR_GITHUB_USER/crud-dd-task-mean-app.git
git branch -M main
git push -u origin main

