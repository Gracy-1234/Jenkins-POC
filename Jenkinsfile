pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'tomcat', url: ''
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
                deploy adapters: [tomcat9(credentialsId: 'tomcat-cred', url: 'http://<EC2_PUBLIC_IP>:8080')], war: 'hello.war'
            }
        }
    }
}
