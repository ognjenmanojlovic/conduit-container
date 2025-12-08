# Conduit Container

A fully containerized deployment of the **Conduit RealWorld App**, consisting of:

- **Angular Frontend**
- **Django REST API Backend**
- **PostgreSQL Database**

This project demonstrates how to containerize a full-stack application, configure services using environment variables, orchestrate multiple containers with Docker Compose, and deploy the entire stack to a remote server as part of the DevSecOps course.

---

## Table of Contents

- [Description](#description)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Quickstart](#quickstart)
  - [Prerequisites](#prerequisites)
- [Deployment (Server)](#deployment-server)
- [Configuration](#configuration)
  - [Environment Variables](#environment-variables)
- [Usage](#usage)
- [Testing checklist](#testing-checklist)
- [Security notes](#security-notes)
- [Author](#author)

---

## Description

The **Conduit Container Project** showcases how to containerize a full-stack application using Docker and Docker Compose.

This setup includes:

- A Django backend (REST API)
- An Angular frontend (served via Nginx)
- A PostgreSQL database for persistent storage

All configuration values are stored in environment variables and can be easily adjusted before deployment.

This project demonstrates:

- Containerization of frontend + backend + database  
- Docker multi-stage builds (Angular + Django)  
- Automated backend startup via `entrypoint.sh`  
- Persistent database volumes  
- Deployment to a remote Linux server  
- Healthchecks for reliable orchestration  

---

## Tech Stack

- **Frontend:** Angular + Node.js + Nginx  
- **Backend:** Django REST Framework + Gunicorn  
- **Database:** PostgreSQL  
- **Containerization:** Docker + Docker Compose  

---

## Project Structure

```
conduit-container/
├─ backend/                    # Django API (Git submodule)
│  ├─ Dockerfile
│  ├─ entrypoint.sh
│  ├─ requirements.txt
│  └─ ...
├─ frontend/                   # Angular Application (Git submodule)
│  ├─ Dockerfile
│  ├─ .dockerignore
│  └─ ...
├─ docker-compose.yaml
├─ example.env
├─ .gitignore
└─ README.md
```

---

## Quickstart

### Prerequisites

Check Docker:

```bash
docker --version
```

Check Docker Compose:

```bash
docker compose version
```

Check Git:

```bash
git --version
```

---

## Deployment (Server)

### 1. Connect to your server

```bash
ssh <username>@<server-ip>
```

### 2. Verify Docker installation

```bash
docker --version
```

```bash
docker compose version
```

### 3. Clone the repository

```bash
git clone --recursive https://github.com/<your-username>/conduit-container.git
```

```bash
cd conduit-container
```

### 4. Prepare environment variables

```bash
cp example.env .env
```

Edit `.env` and configure:

- Backend port  
- Frontend port  
- PostgreSQL credentials  
- Django allowed hosts (required!)  
- Django superuser values  

### 5. Build Docker images

```bash
docker compose build
```

### 6. Start the application

```bash
docker compose up -d
```

### 7. Access in browser

Frontend:

```
http://<server-ip>:8282
```

Backend API:

```
http://<server-ip>:8000/api/articles/
```

Django Admin:

```
http://<server-ip>:8000/admin/
```

### 8. Persistence Test

```bash
docker compose down
```

```bash
docker compose up -d
```

Your created users or articles should still exist.

---

## Configuration

All configuration is done via `.env`.

To create your environment file:

```bash
cp example.env .env
```

---

## Environment Variables

```env
# Ports
FRONTEND_PORT=8282
BACKEND_PORT=8000

# Django allowed hosts (must include your server IP)
DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1,<your.server.ip>

# Database
POSTGRES_DB=conduit
POSTGRES_USER=conduit
POSTGRES_PASSWORD=change_me_password

# Backend DB connection
DB_HOST=db
DB_PORT=5432
DB_NAME=${POSTGRES_DB}
DB_USER=${POSTGRES_USER}
DB_PASSWORD=${POSTGRES_PASSWORD}

# Django Superuser (created automatically on first run)
DJANGO_SUPERUSER_USERNAME=admin
DJANGO_SUPERUSER_EMAIL=admin@example.com
DJANGO_SUPERUSER_PASSWORD=change_me_admin_pass
```

---

## Usage

### Start stack

```bash
docker compose up -d
```

### Stop stack

```bash
docker compose down
```

### Backend logs

```bash
docker compose logs backend
```

### Frontend logs

```bash
docker compose logs frontend
```

### Postgres logs

```bash
docker compose logs db
```

### Access Django admin panel

```
http://<server-ip>:8000/admin/
```

---

## Testing checklist

- [x] Frontend container builds successfully  
- [x] Backend container builds successfully  
- [x] Database container uses persistent volume  
- [x] Backend reachable via `<server-ip>:8000/api/...`  
- [x] Frontend reachable via `<server-ip>:8282`  
- [x] Django admin login works  
- [x] Environment variables loaded correctly  
- [x] All services marked **healthy** via Docker healthchecks  
- [x] `.env` excluded from Git  
- [x] Data persists after restart  

---

## Security notes

- `.env` must **never** be committed  
- Use long, secure passwords  
- Replace `<your.server.ip>` before deployment  
- Do not expose unnecessary ports  
- Keep dependencies updated  

---

## Author

**Ognjen Manojlovic**

- Instagram: https://instagram.com/0gisha  
- LinkedIn: https://www.linkedin.com/in/ognjen-manojlovic