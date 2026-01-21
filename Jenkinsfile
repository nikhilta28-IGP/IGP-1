pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/nikhilta28-IGP/IGP-1.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t abctechnologies:latest .'
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
