pipeline {
    agent any

    environment {
        IMAGE_NAME = 'nodejsdocker01'
        IMAGE_TAG = 'latest'
        DOCKERHUB_CREDENTIALS = 'dockerhub-credentials'  
        DOCKERHUB_USERNAME = 'aryanrajsinghrathore'     
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Aryan-Raj-Singh-Rathore/CloudPlatformAssesment.git'
            }
        }

        stage('Install Node.js Dependencies') {
            steps {
                dir('blogWebsite-main') {
                    bat 'npm install'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('blogWebsite-main') {
                    bat "docker build -t %DOCKERHUB_USERNAME%/%IMAGE_NAME%:%IMAGE_TAG% ."
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS}") {
                        def image = docker.image("${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}")
                        image.push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image pushed to Docker Hub successfully!'
        }
        failure {
            echo 'Build or push failed.'
        }
    }
}
