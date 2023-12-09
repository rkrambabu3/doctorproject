pipeline{
    agent any
    stages{
        stage("code checkou ..."){
            steps{
                git branch: 'main', url: 'https://github.com/rkrambabu3/doctorproject'
            }
        }
        stage("Maven Build Out"){
            steps{
                sh 'mvn package'
            }
        }
        stage("Dev Deploying ..."){
            steps{
                sshagent(['tomcat-dev']) {
                // Copy War file to tomcat dev Server
                sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@3.89.189.191:/opt/tomcat9/webapps/"
                // Restart tomcat server
                sh "ssh ec2-user@52.71.254.233 /opt/tomcat9/bin/shutdown.sh"
                sh "ssh ec2-user@152.71.254.233 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
}
