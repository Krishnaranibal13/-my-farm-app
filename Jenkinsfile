pipeline {
    agent any

    environment {
        APP_DIR = '/home/ubuntu/my-farm-app'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Verify Docker') {
            steps {
                sh '''
                    docker --version
                    docker compose version
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                    cd ${APP_DIR}

                    docker compose build
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    cd ${APP_DIR}

                    docker compose down
                    docker compose up -d
                '''
            }
        }

        stage('Verify Containers') {
            steps {
                sh '''
                    cd ${APP_DIR}

                    docker compose ps
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
