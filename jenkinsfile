pipeline {
    agent any

    environment {
        DOCKER_COMPOSE = "/usr/local/bin/docker-compose" // path to docker-compose
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<YOUR_USERNAME>/<YOUR_REPO>.git'
            }
        }

        stage('Build & Deploy') {
            steps {
                sh "${DOCKER_COMPOSE} down"
                sh "${DOCKER_COMPOSE} up -d --build"
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
