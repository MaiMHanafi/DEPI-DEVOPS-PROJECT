# DEPI DevOps Project

## Overview
This project is part of the Digital Egypt Pioneers Initiative (DEPI) DevOps Track. It involves containerizing a full-stack web application and automating its deployment using Jenkins.

## Features
- **Automated Server Setup with Ansible**: An Ansible playbook is used to install requirements and prerequisites to prepare the machine.
- **Email Notifications via Ansible Role**: An Ansible role is implemented to set up a mail server that sends email notifications after both successful and failed deployments.
- **Frontend & Backend Dockerization**: The application is containerized using Docker for both frontend and backend services.
- **Docker Compose**: A `docker-compose.yml` file is used to orchestrate multi-container deployment.
- **CI/CD with Jenkins**: Automated build, testing, and deployment using Jenkins.

## Technologies Used
- **Configuration Management**: Ansible
- **Database**: MongoDB
- **Backend**: Python (Flask)
- **Frontend**: React.js
- **Containerization**: Docker, Docker Compose
- **CI/CD**: Jenkins
- **Version Control**: GitHub

## Dockerization
### Backend Service
The backend is a Flask application that exposes APIs for the frontend. The `Dockerfile` for the backend:

```Dockerfile
# Use official Python image
FROM python:3.9

# Set working directory
WORKDIR /app

# Copy application files
COPY . .

# Install dependencies
RUN pip install -r requirements.txt

# Expose the application port
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
```

### Frontend Service
The frontend is a React.js application. The `Dockerfile` for the frontend:

```Dockerfile
# Use Node.js image for building the frontend
FROM node:18 as build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Use Nginx for serving the frontend
FROM nginx:alpine as production-stage
COPY --from=build-stage /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## Docker Compose
A `docker-compose.yml` file is used to manage the frontend and backend services:

```yaml
version: '3.8'

services:
  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      - FLASK_ENV=production
    depends_on:
      - db

  frontend:
    build: ./frontend
    ports:
      - "80:80"
    depends_on:
      - backend

  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydatabase
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

## Jenkins CI/CD Pipeline

### Ansible Automation
Ansible is used to automate server setup and deployment notifications:
- **Setup Playbook**: Installs dependencies and prerequisites.
- **Mail Server Role**: Configures an SMTP mail server to send notifications on deployment status.

For more details, check the Ansible configuration repository: [SMT-Configuration](https://github.com/MaiMHanafi/SMT-Configuration.git)
A `Jenkinsfile` is used for automating the build, testing, and deployment process:

```groovy
pipeline {
    agent any
    
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/MaiMHanafi/DEPI-DEVOPS-PROJECT.git'
            }
        }
        
        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'docker-compose run backend pytest tests/'
            }
        }
        
        stage('Deploy Application') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}
```

## Running the Project Locally
1. Clone the repository:
   ```sh
   git clone https://github.com/MaiMHanafi/DEPI-DEVOPS-PROJECT.git
   ```
2. Navigate to the project directory:
   ```sh
   cd DEPI-DEVOPS-PROJECT
   ```
3. Build and start the containers:
   ```sh
   docker-compose up --build -d
   ```
4. Access the application:
   - Backend: `http://localhost:5000`
   - Frontend: `http://localhost`

## Contributing
Contributions are welcome! Feel free to submit issues and pull requests.

## License
This project is licensed under the MIT License.

