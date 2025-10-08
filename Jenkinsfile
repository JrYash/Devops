pipeline {
    agent any

    environment {
        REGISTRY = 'your-dockerhub-username'          // e.g., mydockeruser
        FRONTEND_IMAGE = 'sports-mini-frontend'
        BACKEND_IMAGE = 'sports-mini-backend'
        TAG = 'latest'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend Image') {
            steps {
                script {
                    dir('frontend') {
                        sh "docker build -t ${REGISTRY}/${FRONTEND_IMAGE}:${TAG} ."
                    }
                }
            }
        }

        stage('Build Backend Image') {
            steps {
                script {
                    dir('backend') {
                        sh "docker build -t ${REGISTRY}/${BACKEND_IMAGE}:${TAG} ."
                    }
                }
            }
        }

        stage('Run docker-compose for testing') {
            steps {
                sh 'docker-compose up -d'
                // Simple check to ensure backend API is reachable
                sh 'sleep 5 && curl -f http://localhost:4000/api/sports'
            }
            post {
                always {
                    sh 'docker-compose down'
                }
            }
        }

        stage('Push to Registry') {
            when {
                expression { return env.REGISTRY_CREDENTIALS_ID != null }
            }
            steps {
                script {
                    docker.withRegistry('', "${REGISTRY_CREDENTIALS_ID}") {
                        sh "docker push ${REGISTRY}/${FRONTEND_IMAGE}:${TAG}"
                        sh "docker push ${REGISTRY}/${BACKEND_IMAGE}:${TAG}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
