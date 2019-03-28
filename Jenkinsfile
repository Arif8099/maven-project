pipeline {

    agent any
    
    tools {
        maven 'localMaven'
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

        stage('deploy-to-staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }
        
        stage('deploy-to-prod') {
            steps {
                timeout(time=5,unit='DAYS'){
                    input message:'Approve Production Deployement?'
                }

                build job:'deploy-to-prod'
            }

            post {
                success {
                    echo 'code deployed to production'
                }

                failure {
                    echo 'deployement failed!'
                }
            }
        }
    }
}
