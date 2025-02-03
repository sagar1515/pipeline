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
                    sh 'docker-compose down'  # Stop running containers
                    sh 'docker build -t $IMAGE_NAME -f docker/Dockerfile .'  # Build new image locally
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    sh 'docker-compose up -d'  # Start new containers
                }
            }
        }
    }
}

