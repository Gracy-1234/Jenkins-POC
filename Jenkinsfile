stage('Package WAR') {
    steps {
        sh '''
            rm -f HelloWorld.war
            mkdir -p hello/WEB-INF
            cp index.html hello/
            jar -cvf HelloWorld.war -C hello .
        '''
    }
}

stage('Deploy to Tomcat') {
    steps {
        deploy adapters: [tomcat9(credentialsId: 'Tomcat_creds', url: 'http://13.239.55.245:8080')], war: 'HelloWorld.war'
    }
}
