pipeline {
    agent any

    tools {
        maven 'local maven'
    }

    parameters {
        string(name: 'tomcat_dev', defaultValue: '54.234.74.185', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '54.237.229.123', description: 'Production Server')
    }

    stages {
        stage('Build') {
            steps {
                // Maven clean and package command
                bat 'mvn clean package'
            }
            post {
                success {
                    // Archive the built .war files
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/webapp/target/*.war'
                }
            }
        }

        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        script {
                            // Define variables for staging server
                            def warFilePath = "${env.WORKSPACE}\\webapp\\target\\webapp.war"
                            def sshKeyPath = 'C:\\Users\\wangx\\tomcat\\tomcat-demo.pem'
                            def remoteUser = 'ec2-user'
                            def remoteHost = params.tomcat_dev

                            // Debugging output
                            echo "WAR File Path: ${warFilePath}"
                            echo "SSH Key Path: ${sshKeyPath}"

                            // SCP command to transfer the .war file to the staging server
                            bat """
                            scp -i "${sshKeyPath}" "${warFilePath}" ${remoteUser}@${remoteHost}:/var/lib/tomcat9/webapps/
                            """
                        }
                    }
                }

                stage('Deploy to Production') {
                    steps {
                        script {
                            // Define variables for production server
                            def warFilePath = "${env.WORKSPACE}\\webapp\\target\\webapp.war"
                            def sshKeyPath = 'xxxxxx\\tomcat\\tomcat-demo.pem'
                            def remoteUser = 'ec2-user'
                            def remoteHost = params.tomcat_prod

                            // Debugging output
                            echo "WAR File Path: ${warFilePath}"
                            echo "SSH Key Path: ${sshKeyPath}"

                            // SCP command to transfer the .war file to the production server
                            bat """
                            scp -i "${sshKeyPath}" "${warFilePath}" ${remoteUser}@${remoteHost}:/var/lib/tomcat9/webapps/
                            """
                        }
                    }
                }
            }
        }
    }
}
