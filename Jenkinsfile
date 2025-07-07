pipeline {
    agent any

    environment {
        IMAGE_NAME = 'praveen/devops-pipeline-app'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://gitlab.com/praveenalavala-group/devops-pipeline-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "docker login -u $DOCKER_USER -p $DOCKER_PASS"
                    sh "docker push ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy to Kubernetes with Helm') {
            steps {
                sh """
                    helm upgrade --install myapp helm-chart \
                    --set image.repository=${IMAGE_NAME},image.tag=latest
                """
            }
        }
    }
}
