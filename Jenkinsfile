pipeline{
    agent any
    stages{
        stage("Maven Build"){
            steps{
                sh 'mvn package'
            }
        }
        stage("Deploy Dev"){
            steps{
                sshagent(['tomcat-dev']) {
                    // Copy war file to tomcat web server 
                    sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.34.67:/opt/tomcat10/webapps"
                    // Restart tomcat server
                    sh "ssh ec2-user@172.31.34.67 /opt/tomcat10/bin/shutdown.sh"
                    sh "ssh ec2-user@172.31.34.67 /opt/tomcat10/bin/startup.sh"
                }
            }
        }
    }
}
