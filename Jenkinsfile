pipeline {
     agent any

        tools{
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
       
       stage('Build'){
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
                        withCredentials([file(credentialsId: 'tomcat-demo.pem', variable: 'SSH_KEY')]) {
                            script {
                                def warFilePath = "${env.WORKSPACE}\\target\\*.war"
                                bat """
                                winscp.com /command ^
                                  "open sftp://ec2-user@${params.tomcat_dev}/ -privatekey=\\"$SSH_KEY\\"" ^
                                  "put $warFilePath /var/lib/tomcat9/webapps/" ^
                                  "exit"
                                """
                            }
                        }
                    }
                }

                stage ('Deploy to Production') {
                    steps {
                        withCredentials([file(credentialsId: 'tomcat-demo.pem', variable: 'SSH_KEY')]) {
                            script {
                                def warFilePath = "${env.WORKSPACE}\\target\\*.war"
                                bat """
                                winscp.com /command ^
                                  "open sftp://ec2-user@${params.tomcat_prod}/ -privatekey=\\"$SSH_KEY\\"" ^
                                  "put $warFilePath /var/lib/tomcat9/webapps/" ^
                                  "exit"
                                """
                            }
                        }
                    }
                }
            }
        }
    }
}
