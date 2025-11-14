stage('Package WAR') {
    steps {
        sh '''
            rm -f HelloWorld.war
            mkdir -p hello/WEB-INF
            # Create web.xml to set index.html as welcome file
            cat <<EOF > hello/WEB-INF/web.xml
            <?xml version="1.0" encoding="UTF-8"?>
            <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                     xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                                         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
                                    <welcome-file-list>
                    <welcome-file>index.html</welcome-file>
                </welcome-file-list>
            </web-app>
            EOF
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
