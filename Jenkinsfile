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
        stage('Check Tomcat Status') {
            steps {
                script {
                    // Check if Tomcat is already running
                    def tomcatStatus = sh(script: 'ps aux | grep [t]omcat', returnStatus: true)
                    
                    if (tomcatStatus == 0) {
                        // Tomcat is already running
                        echo 'Tomcat is already running. Skipping startup.'
                    } else {
                        // Tomcat is not running, start it
                        echo 'Tomcat is not running. Starting Tomcat...'
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@52.71.254.233 /opt/tomcat9/bin/startup.sh"
                        // Add your code to start Tomcat here
                    }
                }
            }
        }
        stage("Dev Deploying ..."){
            steps{
                sshagent(['tomcat-dev']) {
                // Copy War file to tomcat dev Server
                sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@52.71.254.233:/opt/tomcat9/webapps/"
                // Restart tomcat server
                // sh "ssh ec2-user@52.71.254.233 /opt/tomcat9/bin/shutdown.sh"
                // sh "ssh ec2-user@52.71.254.233 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
}
