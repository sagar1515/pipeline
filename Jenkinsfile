pipeline {
    agent any

    environment {
        IMAGE_NAME = "laravel-docker"
        CONTAINER_NAME = "laravel_app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sagar1515/pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Stop running containers
                    sh 'docker-compose down'

                    // Build new image locally
                    sh 'docker build -t $IMAGE_NAME -f docker/Dockerfile .'
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    // Start new containers
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}

