pipeline {
    agent any
    tools {
        maven 'localMaven'
        jdk 'localJDK'
    }
    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo "Now Archiving....."
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage('Deploy to Production') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message:'Approve PRODUCTION deployment?'
                }
                build job: 'deploy-to-production'
            }
            post {
                sucess {
                    echo 'Code deployed to Production.'
                }
                failue {
                    echo 'Deployment failed.'
                }
            }
        }
    }
}
