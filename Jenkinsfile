pipeline {
    agent any

    environment {
        IMAGE_NAME = "nikhilta28/abctechnologies"
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {

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
