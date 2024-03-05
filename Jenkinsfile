pipeline{
    agent any
    stages{
        stage("Code Checkout"){
            steps{
                git branch: 'main', credentialsId: 'github-1', url: 'https://github.com/git212/doctor-online'
            }
        }
        stage("Maven Build"){
            steps{
                sh 'mvn package'
            }
        }
        stage("Dev Deploy"){
            steps{
                sshagent(['tomcat-dev']) {
                    // Copy war file to tomcat dev server
                    sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.33.81:/opt/tomcat9/webapps/"
                    //Restart tomcat server
                    sh "ssh ec2-user@172.31.33.81 /opt/tomcat9/bin/shutdown.sh"
                    sh "ssh ec2-user@172.31.33.81 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
}
