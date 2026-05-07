pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-demo"
        CONTAINER_NAME = "flask-container"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/dineshbehera/devops-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Stop Old Container') {
            steps {
                bat '''
                docker stop %CONTAINER_NAME% || exit 0
                docker rm %CONTAINER_NAME% || exit 0
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '''
                docker run -d ^
                --name %CONTAINER_NAME% ^
                -p 5000:5000 ^
                %IMAGE_NAME%
                '''
            }
        }
    }

    post {
        always {
            bat 'docker ps'
        }
    }
}
