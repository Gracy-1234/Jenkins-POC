stage('Package WAR') {
            steps {
                // Create WAR with index.html at root
                sh '''
                rm -rf hello ROOT.war
                mkdir -p hello/WEB-INF
                cp index.html hello/
                jar -cvf ROOT.war -C hello .
                '''
            }
        }
 
        stage('Deploy to Tomcat') {
            steps {
                // Deploy WAR to remote Tomcat using Jenkins plugin
                deploy adapters: [tomcat9(credentialsId: 'Tomcat_creds', url: 'http://13.239.55.245:8080')], war: 'ROOT.war'
            }
        }
    }
