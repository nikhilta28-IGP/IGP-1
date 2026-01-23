pipeline {
    agent any

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "nikhilta28/abctechnologies"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/<your-repo>.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker push ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                export IMAGE_TAG=${IMAGE_TAG}
                cd /var/lib/jenkins/ansible
                ansible-playbook deploy-k8s.yml
                '''
            }
        }
    }
}
