pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "holuphilix/jenkins-pipeline-app:${BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Holuphilix/Jenkins-scm.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'echo "<h1>Welcome to Jenkins Pipeline!</h1>" > index.html'
            }
        }

        stage('Validate Docker') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8080:80 $DOCKER_IMAGE'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
        always {
            sh 'docker system prune -f'
        }
    }
}


