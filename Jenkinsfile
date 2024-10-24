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

    stages{
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
            stage ('Deployments'){
                parallel{
                    stage ('Deploy to Staging'){
                        steps {
                            // bat "winscp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                            bat "winscp -i /Users/wangx/tomcat/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat9/webapps"

                        }
                    }
                    stage ("Deploy to Production"){
                        steps {
                            // bat "winscp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                            bat "winscp -i /Users/wangx/tomcat/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat9/webapps"
                        }
                    }
                }
            }
        }
    }