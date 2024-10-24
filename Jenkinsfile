pipeline {
    agent any

    tools {
        maven 'local maven'
    }
    
    parameters {
        string(name: 'tomcat_dev', defaultValue: '54.237.229.123', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '54.234.74.185', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *') // Polling Source Control
    }

    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments') {
            parallel {
                stage ('Deploy to Staging') {
                    steps {
                        script {
                            def warFilePath = "${env.WORKSPACE}\\target\\*.war"
                            def sshKeyPath = '/c/Users/wangx/tomcat/tomcat-demo.pem'
                            // Debugging output
                            echo "WAR File Path: ${warFilePath}"
                            echo "SSH Key Path: ${sshKeyPath}"
                            bat """
                            winscp.com /command ^
                              "open sftp://ec2-user@${params.tomcat_dev}/ -privatekey=\\"${sshKeyPath}\\" -hostkey="\ssh-ed25519 255 3O5NmtIQi8vcqZqSCSAtHeWbIOMl+EfVTcfLZagXWtg\\"" ^
                              "put ${warFilePath} /var/lib/tomcat9/webapps/" ^
                              "exit"
                            """
                        }
                    }
                }

                stage ('Deploy to Production') {
                    steps {
                        script {
                            def warFilePath = "${env.WORKSPACE}\\target\\*.war"
                            def sshKeyPath = '/c/Users/wangx/tomcat/tomcat-demo.pem'
                            bat """
                            winscp.com /command ^
                              "open sftp://ec2-user@${params.tomcat_prod}/ -privatekey=\\"${sshKeyPath}\\" -hostkey="\ssh-ed25519 255 kuAQCFV4U3CiqezFwRnXvPNO+nj8NPY5mRHbelq+gbo\\"" ^
                              "put ${warFilePath} /var/lib/tomcat9/webapps/" ^
                              "exit"
                            """
                        }
                    }
                }
            }
        }
    }
}
 