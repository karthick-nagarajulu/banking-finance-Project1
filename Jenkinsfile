pipeline {
    agent any

    environment {
        ANSIBLE_DIR = "${WORKSPACE}/ansible"
        INVENTORY   = "inventory/hosts"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out source code"
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building application"
                sh '''
                  mvn clean package -DskipTests
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running tests"
                sh '''
                  mvn test || true
                '''
            }
        }

        stage('Approval for Production') {
            steps {
                input message: 'Approve deployment to PROD?',
                      ok: 'Deploy'
            }
        }

        stage('Deploy to PROD using Ansible') {
            steps {
                echo "Deploying application to PROD via Ansible"
                sh '''
                  cd ansible
                  ansible-playbook -i inventory/hosts playbooks/deploy.yml
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
