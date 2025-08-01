pipeline {
    agent any

    environment {
        IMAGE_NAME = "basicapp"
        DOCKER_REGISTRY = "ghcr.io/rajlaxmirj"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/rajlaxmirj/basicApp.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build --prod'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-docker-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh "docker login ghcr.io -u $USERNAME -p $PASSWORD"
                    sh "docker tag ${IMAGE_NAME} ${DOCKER_REGISTRY}/${IMAGE_NAME}:latest"
                    sh "docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "docker run -d -p 80:80 --name basicapp ${DOCKER_REGISTRY}/${IMAGE_NAME}:latest"
            }
        }
    }
}
