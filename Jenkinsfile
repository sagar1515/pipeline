pipeline {
    agent any

    environment {
        IMAGE_NAME = "laravel-docker"
        CONTAINER_NAME = "laravel_app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {  // ⏳ Stop if checkout takes too long
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],
                        userRemoteConfigs: [[url: 'https://github.com/sagar1515/pipeline.git']],
                        extensions: [[$class: 'CloneOption', depth: 1, noTags: true, shallow: true]]
                    ])
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    timeout(time: 5, unit: 'MINUTES') {
                        sh '''
                        docker-compose down
                        docker build --pull --rm --no-cache -t $IMAGE_NAME -f docker/Dockerfile .
                        '''
                    }
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    timeout(time: 3, unit: 'MINUTES') {
                        sh 'docker-compose up -d'
                    }
                }
            }
        }
    }
}

