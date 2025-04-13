# DEPI DevOps Graduation Project - Group 2 - MERN Task Manager Web Application - Containerized Deployment using Jenkins Pipeline

## Project Overview
This project involves containerizing a MERN (MongoDB, Express, React, Node.js) task management web application and automating its deployment using Jenkins. The application source is based on [Project repository](https://github.com/aayush301/MERN-task-manager.git), while the containerization and automation enhancements are implemented in [pre-requisistes repository](https://github.com/MaiMHanafi/DEPI-DEVOPS-PROJECT.git).

The project provides a **hands-on implementation of a fully automated CI/CD pipeline** using a modern DevOps tech stack. It demonstrates how to seamlessly integrate **Jenkins**, **Git**, **Docker**, **Kubernetes**, and **Ansible** to enable:

- Streamlined software delivery  
- Automated deployments  
- Efficient configuration management  

The pipeline builds and deploys a **Node.js web application** inside containerized environments, with scalability and orchestration managed through Kubernetes. The documented approach is ideal as a **reference architecture** for DevOps practitioners and learners looking to replicate or build on similar infrastructure patterns.

## Features
- **Containerization**: Uses Docker to containerize both frontend and backend services.
- **Orchestration**: Manages multi-container deployment using Docker Compose.
- **CI/CD Automation**: Implements a Jenkins pipeline for automated deployment.
- **Notifications**: Sends email notifications upon successful or failed deployments.


## 🛠️ Technologies Used

- **Jenkins** – For continuous integration and pipeline automation.  
- **Git** – For source code version control and integration with Jenkins.  
- **Docker** – For containerizing the Node.js application, ensuring consistency across environments.  
- **Ansible** – For automating pre-requisites on the managed node.  
- **Kubernetes** – For orchestrating containerized applications and enabling scalability and self-healing deployments.  
- **Node.js** – The backend runtime environment for the application being built and deployed.

---


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

### 5. Kubernetes
This Kubernetes cluster is created using **Minikube**. The cluster consists of the following resources:

- `app-config.yaml`
- `app-secret.yaml`
- `db-persistentvolumeclaim.yaml`
- `db-deployment.yaml`
- `db-service.yaml`
- `backend-deployment.yaml`
- `backend-service.yaml`
- `frontend-deployment.yaml`
- `frontend-service.yaml`

#### To set up the cluster and run the application, follow these commands in order:
```sh
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
kubectl apply -f db-persistentvolumeclaim.yaml
kubectl apply -f db-deployment.yaml
kubectl apply -f db-service.yaml
kubectl apply -f backend-deployment.yaml
kubectl apply -f backend-service.yaml
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
```

### ✅ To verify all resources are running:
```sh
kubectl get all
```

### 🌐 To access the web application in your browser:
Start Minikube and retrieve its IP:

```sh
minikube start
```
then access frontend via:
```sh
http://<minikube-ip>:30080/
```
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
