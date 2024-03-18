pipeline {
    agent any
    stages{
        stage("CodeCheckout"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/git212/doctor-online'
            }
        }
        stage("Maven Build"){
            steps{
                sh 'mvn package'
            }
        }
        stage("Deploy Dev"){
            steps{
               sshagent(['tomcat-dev']) {
                  // Copy war file to tomcat dev server
                  sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.38.129:/opt/tomcat9/webapps/"
                  // Restart tomcat server
                  sh "ssh ec2-user@172.31.38.129 /opt/tomcat9/bin/shutdown.sh"
                  sh "ssh ec2-user@172.31.38.129 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
}
