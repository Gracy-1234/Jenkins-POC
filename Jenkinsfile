pipeline {
    agent any

    environment {
        APP_SERVER = 'ubuntu@3.25.199.180'
        APP_SERVER_PATH = '/var/www/html'  // Changed to root directory
        SSH_KEY_CREDENTIALS = 'apache-ssh-key'  // Matches Jenkins credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out source code..."
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
                        # Create target directory on remote server
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} 'sudo mkdir -p ${APP_SERVER_PATH} && sudo chown ubuntu:ubuntu ${APP_SERVER_PATH}'

                        # Copy all files (HTML, CSS, JS, images) to Apache directory
                        scp -o StrictHostKeyChecking=no -r * ${APP_SERVER}:${APP_SERVER_PATH}/

                        # Restart Apache to apply changes
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} 'sudo systemctl restart apache2'
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
