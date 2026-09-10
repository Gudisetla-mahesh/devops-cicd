pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-cicd-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Gudisetla-mahesh/devops-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ./app'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'minikube image load ${IMAGE_NAME}:${IMAGE_TAG}'
                sh 'sed "s|image: devops-cicd-app:v1|image: devops-cicd-app:${IMAGE_TAG}|" k8s/deployment.yml > /tmp/deployment-${BUILD_NUMBER}.yml'
                sh 'kubectl apply -f /tmp/deployment-${BUILD_NUMBER}.yml'
                sh 'kubectl rollout status deployment/devops-cicd-app --timeout=120s'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get svc devops-cicd-service'
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully!'
        }
        failure {
            echo 'CI/CD pipeline failed.'
        }
    }
}
