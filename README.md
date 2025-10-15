# Fusionpact DevOps Gauntlet Challenge

**Candidate:** Arpit Choudhary
**Submission Date:** October 15, 2025

This repository contains the complete solution for the DevOps Internship technical challenge by Fusionpact. The project demonstrates a full CI/CD workflow for a multi-service application, including containerization, automated builds, deployment to a cloud server, and monitoring.

---

## 🚀 Deployed Application

The live application has been successfully deployed and is accessible at the following endpoints:

* **Frontend Application**: `http://<your-ec2-public-ip>:8080`
* **Backend API**: `http://<your-ec2-public-ip>:8000`
* **Prometheus Dashboard**: `http://<your-ec2-public-ip>:9090`



---

## 🛠️ Technology Stack

* **Cloud Provider**: AWS EC2
* **Containerization**: Docker & Docker Compose
* **CI/CD**: GitHub Actions
* **Container Registry**: Docker Hub
* **Backend**: Python (FastAPI)
* **Frontend**: Nginx
* **Monitoring**: Prometheus

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
│   └── ...               # Static HTML, CSS, JS files
├── prometheus/
│   └── prometheus.yml    # Prometheus configuration
├── docker-compose.yml    # Defines services for production deployment
└── README.md             # Project documentation
```

---

## 🔄 CI/CD Pipeline Overview

The project is configured with a robust CI/CD pipeline using GitHub Actions, which automates the entire build and deployment process.



### 1. Build and Push (`build-and-push`)

* **Trigger**: This job runs automatically on every `git push` to the `main` branch.
* **Actions**:
    1.  Logs into Docker Hub using credentials stored in GitHub Secrets.
    2.  Builds separate Docker images for the `backend` and `frontend` services using their respective Dockerfiles.
    3.  Pushes the newly built images to the Docker Hub registry with a `:latest` tag, making them ready for deployment.

### 2. Deploy (`deploy`)

* **Trigger**: This job runs only after the `build-and-push` job succeeds.
* **Actions**:
    1.  Securely connects to the production AWS EC2 instance via SSH using a private key stored in GitHub Secrets.
    2.  Navigates to the project directory on the server.
    3.  Pulls the latest changes from the GitHub repository to ensure the `docker-compose.yml` is up-to-date.
    4.  Runs `docker-compose pull` to download the new images that were just pushed to Docker Hub.
    5.  Restarts all services using `docker-compose up -d --remove-orphans` to apply the changes without downtime for other services.

---

## ⚙️ How to Run Locally

To set up and run this project on a local machine, you need Docker and Docker Compose installed.

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/arpitt29/fusionpact-devops-challenge.git](https://github.com/arpitt29/fusionpact-devops-challenge.git)
    cd fusionpact-devops-challenge
    ```

2.  **Modify `docker-compose.yml` for local builds:**
    For local testing, the `docker-compose.yml` file needs to be temporarily modified. Change the `backend` and `frontend` service definitions from `image: ...` to `build: ./{service_name}`.

3.  **Build and run the containers:**
    This command will build the images directly from the local Dockerfiles and start all services.
    ```bash
    docker-compose up --build -d
    ```
    The application will then be accessible at `http://localhost:8080`.

---

## 💾 Data Persistence Verification

Data persistence for the Prometheus monitoring service is achieved using a **Docker named volume**. In the `docker-compose.yml` file, the `prometheus-data` volume is mounted to the `/prometheus` directory inside the container.

This was verified by:
1.  Running the application and allowing Prometheus to collect data.
2.  Stopping and removing all containers with `docker-compose down`.
3.  Restarting the application with `docker-compose up -d`.
4.  Confirming that the historical metrics data was still present in the Prometheus dashboard, proving it was successfully persisted on the host machine's volume, independent of the container's lifecycle.