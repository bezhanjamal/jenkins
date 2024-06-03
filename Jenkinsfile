pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Use the ID of your Docker Hub credentials
        DOCKERHUB_REPO = 'bezhan759/assignment'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/bezhanjamal/jenkins.git'
            }
        }

        stage('Build') {
            steps {
                bat 'C:\Users\snabe\AppData\Local\Programs\Python\Python312\python.exe -m venv venv'
                bat '.\\venv\\Scripts\\pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                bat '.\\venv\\Scripts\\python -m unittest discover'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.DOCKERHUB_REPO}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
