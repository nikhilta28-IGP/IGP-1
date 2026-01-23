pipeline {
    agent any

    environment {
        IMAGE_NAME = "nikhilta28/abctechnologies"
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build Application (Maven)') {
            steps {
                sh '''
                mvn clean package
                '''
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
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
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
