@Library("jhc-libs") _

pipeline {
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

    stages {
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage("Upload Artifacts"){
            steps{
               script{
                    def pom = readMavenPom file: 'pom.xml'
                    def version = pom.version
                    def repoName = version.endsWith("SNAPSHOT") ? "do-snapshot": "do-release"
                    nexusArtifactUploader artifacts: [[artifactId: 'doctor-online', classifier: '', file: 'target/doctor-online.war', type: 'war']], 
                        credentialsId: 'nexus3', 
                        groupId: 'in.javahome', 
                        nexusUrl: '172.31.47.53:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: repoName, 
                        version: version
               }
            }
        }
        stage("Tomcat Dev Deploy"){
            steps{
                tomcatDeploy("172.31.30.174","ec2-user","tomcat-dev","doctor-online.war")
            }
        }
    }
    post {
      success {
        cleanWs()
      }
    }
}
