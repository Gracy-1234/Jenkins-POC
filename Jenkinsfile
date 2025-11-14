pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'tomcat', url: 'https://github.com/Gracy-1234/Jenkins-POC.git'
            }
        }

        stage('Package WAR') {
            steps {
                sh '''
                mkdir -p hello/WEB-INF
                cp index.html hello/
                jar -cvf hello.war -C hello .
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'Tomcat_creds', url: 'http://13.239.55.245:8080')], war: 'hello.war'
            }
        }
    }
}
