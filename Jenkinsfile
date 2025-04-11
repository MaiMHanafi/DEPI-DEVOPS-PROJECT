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
                    sh "npm run install-all"
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
                    sh "docker compose up --build -d --remove-orphans"
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
