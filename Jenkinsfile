pipeline{
    agent any
    tools{
        maven 'maven3'
    }
    stages{
        stage("Git Clone/Pull")}
    {
        stage("Maven Build"){
            steps{
                sh 'mvn clean package'
            }
        }
        stage("Deploy to tomacat8-9am"){
            steps{
                sshagent(['tomacat8-9am']) {
                    // rename the war file
                    sh "mv target/*war target/myweb.war"
                    // copy war file to tomcat server
                    sh "scp -o StrictHostKeyChecking=no target/myweb.war ec2-user@http://65.2.83.103:/opt/tomcat8/webapps"
                    // stop and start tomcat
                    sh "ssh ec2-user@http://65.2.83.103 /opt/tomcat8/bin/shutdown.sh"
                    sh "ssh ec2-user@http://65.2.83.103 /opt/tomcat8/bin/startup.sh"
                     stage('Upload to Nexus'){
            steps{
                nexusArtifactUploader artifacts: [
                        [artifactId: 'sample pipeline', classifier: '', file: 'myweb', type: 'war']], 
                    credentialsId: 'nexus3', 
                    groupId: 'devops.shaikmoula', 
                    nexusUrl: 'http://52.66.233.131:8081', 
                    nexusVersion: 'nexus3',protocol: 'http', 
                    repository: 'shaikmoula-devops-snapshot', 
                    version: '1.0-SNAPSHOT'
                }
            }
        }
    }
}


