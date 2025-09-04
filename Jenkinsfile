pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/JrYash/Devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t user-data-app .'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh '''
                    docker stop userapp || true
                    docker rm userapp || true
                    docker run -d --name userapp -p 3000:3000 user-data-app
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
