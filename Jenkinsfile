
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
                sh 'docker build -t my-python-app:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    docker rm -f my-python-app || true
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker run -d \
                      --name my-python-app \
                      --restart unless-stopped \
                      -p 5000:5000 \
                      my-python-app:latest
                '''
            }
        }

        stage('Test Application') {
            steps {
                sh '''
                    sleep 3
                    curl --fail http://localhost:5000/
                '''
            }
        }
    }
}
