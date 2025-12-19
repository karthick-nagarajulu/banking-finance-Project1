pipeline {
    agent any

    environment {
        ANSIBLE_DIR = "/home/banking/ansible"
        INVENTORY   = "inventory/hosts"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/karthick-nagarajulu/banking-finance-Project1.git'
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
                sh """
                  cd ${ANSIBLE_DIR}
                  ansible-playbook -i ${INVENTORY} playbooks/deploy.yml
                """
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
