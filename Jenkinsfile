pipeline {
    agent any

    environment {
        IMAGE_NAME  = "nikhilta28/abctechnologies"
        IMAGE_TAG   = "${BUILD_NUMBER}"
        ANSIBLE_DIR = "/var/lib/jenkins/ansible"
        KUBECONFIG  = "/var/lib/jenkins/.kube/config"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/nikhilta28-IGP/IGP-1.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'mydockerhubcred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                    echo "\$DOCKER_PASS" | docker login -u "\$DOCKER_USER" --password-stdin
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Deploy to Docker via Ansible') {
            steps {
                sh """
                cd ${ANSIBLE_DIR}
                source /var/lib/jenkins/ansible-venv/bin/activate
                ansible-playbook -i inventory deploy-docker.yml
                """
            }
        }

        stage('Deploy to Kubernetes via Ansible') {
            steps {
                sh """
                cd ${ANSIBLE_DIR}
                source /var/lib/jenkins/ansible-venv/bin/activate
                ansible-playbook -i inventory k8s-deploy.yml
                """
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD Pipeline executed successfully!"
        }
        failure {
            echo "❌ CI/CD Pipeline failed. Check Jenkins logs!"
        }
    }
}
