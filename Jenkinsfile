pipeline {
    agent any

    environment {
        IMAGE_NAME = "nikhilta28/abctechnologies"
        IMAGE_TAG  = "17"  // You can replace with ${BUILD_NUMBER} for dynamic tags
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/nikhilta28-IGP/IGP-1.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Docker Login & Push') {
            steps {
                // Using Jenkins credentials for Docker login (PAT)
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',  // <-- your Jenkins credential ID
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

        stage('Deploy using Ansible') {
            steps {
                sh '''
                cd /var/lib/jenkins/ansible
                ansible-playbook -i inventory deploy-docker.yml
                '''
            }
        }
    }
}
