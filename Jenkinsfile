pipeline {
    agent any

    environment {
        IMAGE_NAME = "nikhilta28/abctechnologies"
        IMAGE_TAG  = "17"
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
