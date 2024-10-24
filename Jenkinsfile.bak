pipeline {
    agent any
    tools {
        maven 'local maven'
    }
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Start artifacts....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to staging') {
            steps {
                build job:'deploy-to-staging'
            }
        }
        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Deploy to production environment?' 
                }

                build job: 'deploy-to-production'
            }
            post {
                success {
                    echo 'Successfull deloy to production.'
                }

                failure {
                    echo 'Deploy Falure!'
                }
            }
        }


    }
}