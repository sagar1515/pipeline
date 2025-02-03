pipeline {
    agent any

    environment {
        IMAGE_NAME = "laravel-docker"
        CONTAINER_NAME = "laravel_app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {  // ⏳ Stop if clone takes too long
                    git branch: 'main', url: 'https://github.com/sagar1515/pipeline.git', shallow: true
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    timeout(time: 5, unit: 'MINUTES') {  // ⏳ Stop if build takes too long
                        sh '''
                        docker-compose down  # Only stop old containers, keep volumes
                        docker build --pull --rm --no-cache -t $IMAGE_NAME -f docker/Dockerfile .  # Optimized build
                        '''
                    }
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    timeout(time: 3, unit: 'MINUTES') {  // ⏳ Stop if deploy hangs
                        sh 'docker-compose up -d'
                    }
                }
            }
        }
    }
}

