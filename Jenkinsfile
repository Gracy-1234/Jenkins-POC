pipeline {
    agent any

    stages {
        stage('Package WAR') {
            steps {
                sh '''
                    # Remove old WAR if exists
                    rm -f HelloWorld.war
                    
                    # Create folder structure
                    mkdir -p hello/WEB-INF
                    
                    # Copy index.html
                    cp index.html hello/
                    
                    # Create WAR file
                    jar -cvf HelloWorld.war -C hello .
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'Tomcat_creds', url: 'http://13.239.55.245:8080')], war: 'HelloWorld.war'
            }
        }
    }
}
