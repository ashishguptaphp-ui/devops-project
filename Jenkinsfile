pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'

                sh '''
                    docker build \
                        -t devops-project:latest \
                        .
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Docker image...'

                sh '''
                    docker run -d \
                        --name devops-test \
                        devops-project:latest

                    sleep 5

                    docker ps

                    docker stop devops-test
                    docker rm devops-test
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'

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
            echo '================================='
            echo 'CI/CD DEPLOYMENT SUCCESSFUL'
            echo '================================='
        }

        failure {
            echo '================================='
            echo 'CI/CD DEPLOYMENT FAILED'
            echo '================================='
        }
    }
}