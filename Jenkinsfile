pipeline {
    agent any

    environment {
       registryCredential = 'dockerhub_id'
        registry = "bezhan759/assignment"
        PATH = 'C:\\WINDOWS\\SYSTEM32;C:\\Users\\snabe\\AppData\\Local\\Programs\\Python\\Python312;%PATH%;C:\\"Program Files"\\Docker\\Docker\\resources\\bin;C:\\"Program Files"\\Git\\bin'
    
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/bezhanjamal/jenkins.git'
            }
        }

        stage('Build') {
            steps {
                bat 'python -m venv venv'
                bat '.\\venv\\Scripts\\pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
               bat '.\\venv\\Scripts\\python -m unittest discover -s tests -p "test_app.py"'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.registry}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
dockerImage.push()
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
