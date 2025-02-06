pipeline {
    agent any  // This will run the pipeline on any available Jenkins agent

    environment {
        IMAGE_NAME = "laravel-docker"
        CONTAINER_NAME = "laravel_app"
        DOCKER_COMPOSE_PATH = "./docker-compose.yml"  // Ensure docker-compose.yml is in the root directory
    }

    stages {
        stage('Check Environment') {
            steps {
                script {
                    echo "Checking environment variables and Git installation..."
                    sh 'echo $PATH'
                    sh 'which git'
                    sh 'git --version'
                }
            }
        }

        stage('Checkout Code') {
            steps {
                script {
                    echo "Cloning repository..."
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],
                        userRemoteConfigs: [[url: 'https://github.com/sagar1515/pipeline.git']]
                    ])
                }
            }
        }

        stage('Install PHP Dependencies') {
            steps {
                script {
                    echo "Installing PHP dependencies using Composer..."
                    sh 'composer install --no-interaction --prefer-dist --optimize-autoloader'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image for Laravel app..."
                    sh 'docker-compose down'  // Stop running containers before rebuilding
                    sh 'docker-compose build --no-cache'  // Build the Docker image
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    echo "Starting Docker containers..."
                    sh 'docker-compose up -d'  // Start the containers in detached mode
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Your Laravel site is live at https://pipeline.iqonic.design"
        }
        failure {
            echo "Deployment failed. Check Jenkins logs for more details."
        }
    }
}

