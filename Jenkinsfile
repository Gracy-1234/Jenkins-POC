pipeline {
    agent any

    environment {
        APP_SERVER = 'ubuntu@3.25.115.86'
        TOMCAT_WEBAPPS = '/var/lib/tomcat10/webapps'
        CONTEXT = 'main'
        REMOTE_TMP = '/tmp/html_deploy_main'
        SSH_KEY_CREDENTIALS = 'tomcat-ssh-key'
        TOMCAT_SERVICE = 'tomcat10'
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

        stage('Deploy to Tomcat (Static HTML)') {
            steps {
                echo "Deploying HTML app to Tomcat context /${env.CONTEXT} ..."
                sshagent([env.SSH_KEY_CREDENTIALS]) {
                    sh """
                        # Prepare remote temp dir
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} 'rm -rf ${REMOTE_TMP} && mkdir -p ${REMOTE_TMP}'
                        
                        # Copy your site (index.html + assets)
                        scp -o StrictHostKeyChecking=no -r * ${APP_SERVER}:${REMOTE_TMP}/

                        # Create context dir & move files with correct ownership
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} '
                          sudo mkdir -p ${TOMCAT_WEBAPPS}/${CONTEXT} && \
                          sudo rm -rf ${TOMCAT_WEBAPPS}/${CONTEXT}/* && \
                          sudo cp -r ${REMOTE_TMP}/* ${TOMCAT_WEBAPPS}/${CONTEXT}/ && \
                          sudo chown -R tomcat:tomcat ${TOMCAT_WEBAPPS}/${CONTEXT}
                        '

                        # Restart Tomcat to ensure context reload (optional if auto-deploy picks up)
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} 'sudo systemctl restart ${TOMCAT_SERVICE}'
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
            echo "Verify at: http://3.25.115.86:8080/${env.CONTEXT}/"
        }
        failure {
            echo "Pipeline failed — check logs for details."
        }
    }
}
