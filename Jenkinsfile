pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/my-webapp:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']], 
                          extensions: [], 
                          userRemoteConfigs: [[url: 'https://github.com/Holuphilix/jenkins-scm.git']]
                ])
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -itd -p 8081:80 $DOCKER_IMAGE'
                }
            }
        }
    }
}

