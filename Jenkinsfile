pipeline {
    agent any
 
    stages {
        stage('Deploy HTML') {
            steps {
                sh 'cp index.html /opt/tomcat/webapps/ROOT/index.html'
            }
        }
    }
}
