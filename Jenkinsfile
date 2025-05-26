pipeline {
    agent any

    environment {
        IMAGE_NAME = "pravalikaa18/python-cicd-app"
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Pravalikaa18/python-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                   sh 'docker build -t "${IMAGE_NAME}" .'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'Dockerhub') {
                        docker.image("${IMAGE_NAME}").push("latest")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
                }
            }
        }
    }
}
