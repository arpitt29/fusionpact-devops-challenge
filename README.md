# Fusionpact DevOps Gauntlet Challenge - Solution

**Candidate:** Arpit Choudhary
**Submission Date:** October 15, 2025

This repository contains the complete solution for the DevOps Internship technical challenge. It demonstrates a production-ready, four-tier application stack deployed on AWS, featuring a complete CI/CD workflow, containerization, and a robust monitoring setup with Prometheus and Grafana.

---

## 🚀 Live Application Endpoints

The live application has been successfully deployed and is accessible at the following URLs:

* **Frontend**: `http://<your-ec2-public-ip>:8080`
* **Backend API**: `http://<your-ec2-public-ip>:8000/docs`
* **Prometheus**: `http://<your-ec2-public-ip>:9090`
* **Grafana**: `http://<your-ec2-public-ip>:3000` (Login: `admin`/`admin`)



---

## 🛠️ Technology Stack

| Category              | Technology                                   |
| --------------------- | -------------------------------------------- |
| **Cloud Provider** | AWS EC2                                      |
| **Containerization** | Docker & Docker Compose                      |
| **CI/CD** | GitHub Actions                               |
| **Container Registry**| Docker Hub                                   |
| **Backend** | Python (FastAPI)                             |
| **Frontend** | Nginx                                        |
| **Monitoring** | Prometheus & Grafana                         |

---

## 📁 Project Structure

```
.
├── .github/workflows/
│   └── ci-cd.yaml        # GitHub Actions CI/CD Pipeline
├── backend/
│   ├── app/
│   │   └── main.py       # FastAPI application code
│   ├── Dockerfile        # Backend Docker image instructions
│   └── requirements.txt  # Python dependencies
├── frontend/
│   ├── Dockerfile        # Frontend Docker image instructions
│   └── ...               # Static HTML/CSS files
├── prometheus/
│   └── prometheus.yml    # Prometheus scrape configurations
└── docker-compose.yml    # Defines all services for production
```

---

## 🔄 CI/CD Pipeline Overview

The project is configured with a CI/CD pipeline using GitHub Actions that automates the entire build and deployment process.

### 1. Build and Push (`build-and-push`)
* **Trigger**: Automatically runs on every `git push` to the `main` branch.
* **Actions**:
    1.  Builds separate Docker images for the `backend` and `frontend` services.
    2.  Pushes the versioned images to the Docker Hub registry, making them available for deployment.

### 2. Deploy (`deploy`)
* **Trigger**: Runs only after the `build-and-push` job succeeds.
* **Actions**:
    1.  Securely connects to the production AWS EC2 instance via SSH.
    2.  Pulls the latest changes from the GitHub repository to get the updated `docker-compose.yml`.
    3.  Runs `docker-compose pull` to download the new images from Docker Hub.
    4.  Restarts all services using `docker-compose up -d` to apply the changes.

---

## 📊 Monitoring & Observability

The observability stack consists of Prometheus and Grafana, providing deep insights into both application and infrastructure performance.

* **Prometheus**: Configured to automatically scrape metrics from the backend's `/metrics` endpoint.
* **Grafana**: Visualizes the data collected by Prometheus. It's set up with:
    * **A Prometheus Data Source** pointing to the Prometheus container.
    * **An Infrastructure Dashboard** to monitor server health (CPU, Memory, Disk).
    * **An Application Dashboard** to monitor backend performance (request rates, latency, etc.).

---

## 💾 Data Persistence Verification

Data persistence for stateful services is achieved using **Docker named volumes**, as defined in the `docker-compose.yml` file. This ensures that critical data is not lost when containers are recreated or updated.

* **Prometheus**: The `prometheus-data` volume is mounted to `/prometheus` to store all collected time-series metrics.
* **Grafana**: The `grafana-data` volume is mounted to `/var/lib/grafana` to store all dashboards, data sources, and user configurations.

This was verified by restarting the services and confirming that all historical data and dashboard configurations remained intact.