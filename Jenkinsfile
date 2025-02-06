pipeline {
    agent any

    environment {
        IMAGE_NAME = "laravel-docker"
        CONTAINER_NAME = "laravel_app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/sagar1515/pipeline.git']],
                    extensions: [[$class: 'CloneOption', depth: 1, noTags: true, shallow: true]]
                ])
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'composer install --no-interaction --prefer-dist --optimize-autoloader'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    docker-compose down
                    docker build -t $IMAGE_NAME -f docker/Dockerfile .
                    '''
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}

