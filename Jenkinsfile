pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo '开始存档....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}