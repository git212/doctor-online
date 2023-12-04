pipeline {
    agent any
    stages{
        stage("code checkout "){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/javahometech/doctor-online'
            }
        }
        stage("Maven Build "){
            steps{
                sh 'mvn package'
            }
        }
        stage("Dev Deploy"){
            steps{
                sshagent(['tomcat-dev']) {
                  // copy war file to tomcat dev server
                  sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.40.81:/opt/tomcat9/webapps/"
                  // Restart tomcat server
                  sh "ssh ec2-user@172.31.40.81 /opt/tomcat9/bin/shutdown.sh"
                  sh "ssh ec2-user@172.31.40.81 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
}
