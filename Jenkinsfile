pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                    -t devops-project:latest \
                    .
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    docker images devops-project
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop devops-app || true
                    docker rm devops-app || true

                    docker run -d \
                        --name devops-app \
                        -p 8080:80 \
                        devops-project:latest
                '''
            }
        }

    }

    post {

        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }

    }
}