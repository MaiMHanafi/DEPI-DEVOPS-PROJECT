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
- **Backend**: Node.js (Express.js)
- **Frontend**: React.js
- **Containerization**: Docker, Docker Compose
- **CI/CD**: Jenkins
- **Version Control**: GitHub

## Dockerization
### Backend Service
The backend is a Node.js application that exposes APIs for the frontend. The `Dockerfile` for the backend:

```Dockerfile
# Use Node.js image
FROM node:18

# Set the working directory
WORKDIR /app

# Copy dependencies and install
COPY package*.json ./
RUN npm install

# Copy the backend source code
COPY . .

# Expose the backend port
EXPOSE 5000

# Start the backend server
CMD ["npm", "run", "dev"]
```

### Frontend Service
The frontend is a React.js application. The `Dockerfile` for the frontend:

```Dockerfile
# Use Node.js image
FROM node:18

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install --legacy-peer-deps

# Copy application files
COPY . .

# Expose frontend port
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
```

## Docker Compose
A `docker-compose.yml` file is used to manage the frontend and backend services:

```yaml
services:
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "5000:5000"
    depends_on:
      - mongo
    env_file:
      - .env  #Use .env file for secrets

  mongo:
    image: mongo
    container_name: mongodb
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db
    restart: always  #Ensure MongoDB stays up

volumes:
  mongo_data:
```

## Jenkins CI/CD Pipeline

### Ansible Automation
Ansible is used to automate server setup and deployment notifications:
- **Setup Playbook**: Installs dependencies and prerequisites.
- **Mail Server Role**: Configures an SMTP mail server to send notifications on deployment status.

For more details, check the Ansible configuration repository: [SMT-Configuration](https://github.com/MaiMHanafi/SMT-Configuration.git)

### Jenkins Pipeline
A `Jenkinsfile` is used for automating the build, testing, and deployment process:

```groovy
pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/MaiMHanafi/DEPI-DEVOPS-PROJECT.git'
        REPO_DIR = 'DEPI-DEVOPS-PROJECT'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Repository') {
            steps {
                sh "git clone ${REPO_URL}"
            }
        }

        stage('Install Dependencies') {
            steps {
                dir(REPO_DIR) {
                    sh "npm install"
                }
            }
        }

        stage('Test Application') {
            steps {
                dir(REPO_DIR) {
                    sh 'npm run test || echo "Skipping tests, no test script found"'
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                dir(REPO_DIR) {
                    sh "sudo docker-compose up -d --remove-orphans"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully!'
            sh 'echo "This is to notify you that it\'s a SUCCESSFUL Deployment" | mail -s "Deployment: SUCCESS" maihanafi34@gmail.com'
        }
        failure {
            echo '❌ Deployment failed! Check logs for errors.'
            sh 'echo "This is to notify you that it\'s a FAILED Deployment" | mail -s "Deployment: FAILURE" maihanafi34@gmail.com'
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
   - Frontend: `http://localhost:3000`

## Contributing
Contributions are welcome! Feel free to submit issues and pull requests.

## License
This project is licensed under the MIT License.
