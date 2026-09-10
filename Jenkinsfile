pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Gudisetla-mahesh/devops-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-cicd-app:${BUILD_NUMBER} ./app'
            }
        }

        stage('Test Docker Image') {
            steps {
                sh 'docker images devops-cicd-app'
            }
        }
    }
}
