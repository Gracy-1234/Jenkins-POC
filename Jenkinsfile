pipeline {
    agent any

    environment {
        APP_SERVER = 'ubuntu@<Apache_Server_IP>'
        APP_SERVER_PATH = '/var/www/html/main'
        SSH_KEY_CREDENTIALS = 'apache-ssh-key'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Build successful for HTML app."
            }
        }

        stage('Test') {
            steps {
                echo "Quick code validation completed."
            }
        }

        stage('Deploy to Apache') {
            steps {
                echo "Deploying HTML app to remote Apache server..."
                sshagent([env.SSH_KEY_CREDENTIALS]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} 'mkdir -p ${APP_SERVER_PATH}'
                        scp -o StrictHostKeyChecking=no index.html ${APP_SERVER}:${APP_SERVER_PATH}/index.html
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed — check logs for details."
        }
    }
}
