pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'holuphilix/jenkins-pipeline-app'
        DOCKER_CREDENTIALS_ID = 'holuphilix'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Holuphilix/Jenkins-scm.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                
                script {
                    docker.withRegistry('', 'holuphilix') {
                      sh 'docker push $DOCKER_IMAGE'
                    }
                }
 
            }
        }

        stage('Deploy Container Locally') {
            steps {
                sh 'docker run -d -p 8090:80 $DOCKER_IMAGE'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}