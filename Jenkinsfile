pipeline {
    agent any

    environment {
        APP_SERVER = 'ubuntu@3.25.115.86'
        TOMCAT_WEBAPPS = '/apache-tomcat-10.1.49/webapps'
        CONTEXT = 'main'
        SSH_KEY_CREDENTIALS = 'tomcat-ssh-key'
        TOMCAT_SERVICE = 'apache-tomcat-10.1.49'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                sshagent([env.SSH_KEY_CREDENTIALS]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} 'rm -rf ${TOMCAT_WEBAPPS}/${CONTEXT} && mkdir -p ${TOMCAT_WEBAPPS}/${CONTEXT}'
                        scp -o StrictHostKeyChecking=no -r * ${APP_SERVER}:${TOMCAT_WEBAPPS}/${CONTEXT}/
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} 'sudo chown -R tomcat:tomcat ${TOMCAT_WEBAPPS}/${CONTEXT} && sudo systemctl restart ${TOMCAT_SERVICE}'
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployed successfully! Check: http://3.25.115.86:8080/main/"
        }
        failure {
            echo "Deployment failed."
        }
    }
}
