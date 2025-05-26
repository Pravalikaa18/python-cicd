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
                    withCredentials([usernamePassword(credentialsId: 'Dockerhub' , usernameVariable: 'DOCKER_USER' , passwordVariable: 'DOCKER_PASS')]){
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh "docker push ${env.IMAGE_NAME}:$(env.IMAGE_TAG}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                    withCredentials([file(credentialsId: 'kubeconfig' , variable: 'KUBECONFIG')]) {
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
                
            }
        }
    }
}
