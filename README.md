# DEPI DevOps Graduation Project - Group 2 - MERN Task Manager Web Application - Containerized Deployment using Jenkins Pipeline

## Project Overview
This project involves containerizing a MERN (MongoDB, Express, React, Node.js) task management web application and automating its deployment using Jenkins. The application source is based on [this repository](https://github.com/aayush301/MERN-task-manager.git), while the containerization and automation enhancements are implemented in [this repository](https://github.com/MaiMHanafi/DEPI-DEVOPS-PROJECT.git).

## Features
- **Containerization**: Uses Docker to containerize both frontend and backend services.
- **Orchestration**: Manages multi-container deployment using Docker Compose.
- **CI/CD Automation**: Implements a Jenkins pipeline for automated deployment.
- **Notifications**: Sends email notifications upon successful or failed deployments.

## Project Structure
```
DEPI-DEVOPS-PROJECT/
│-- backend/
│   ├── Dockerfile
│-- frontend/
│   ├── Dockerfile
│-- docker-compose.yml
│-- Jenkinsfile
│-- README.md
```

### 1. Backend Service
- **Dockerfile**: Defines the container for the Node.js backend application.
- **Exposes necessary ports** and includes dependencies.
- **Uses environment variables** for database configuration.

### 2. Frontend Service
- **Dockerfile**: Defines the React application’s container.

### 3. Docker Compose
- **docker-compose.yml**:
  - Defines services for both frontend and backend.
  - Manages dependencies such as MongoDB.
  - Establishes communication between services.

### 4. CI/CD Pipeline (Jenkins)
- **Jenkinsfile**:
  - Pulls the latest code from the repository.
  - Builds and tags Docker images.
  - Deploys the application using Docker Compose.
  - Do the Tests.
  - Sends email notifications for deployment statusin both cases of Success and Failure.

## Setup & Deployment
### Prerequisites
- use this Ansible Playbook in this repo:https://github.com/MaiMHanafi/Pre-requisites-SMT-Configuration.git to install all the pre-requisites.
- **Docker & Docker Compose** installed on the host machine.
- **Jenkins** with necessary plugins for pipeline execution.
- **Configured SMTP settings** for email notifications.

### Steps to Run Locally
1. Clone the repository:
   ```sh
   git clone https://github.com/MaiMHanafi/DEPI-DEVOPS-PROJECT.git
   cd DEPI-DEVOPS-PROJECT
   ```
2. Build and start the services:
   ```sh
   docker-compose up --build -d
   ```
3. Access the application:
   - **Frontend**: `http://localhost:3000`
   - **Backend API**: `http://localhost:5000`

### CI/CD Pipeline Execution
1. Configure Jenkins to pull from this repository.
2. Ensure Docker and Compose are installed on the Jenkins server.
3. Execute the Jenkins pipeline.
4. Monitor email notifications for success or failure.

## Contribution
Feel free to fork the project and contribute improvements. Submit a pull request for review.

## License
This project is licensed under the MIT License.

