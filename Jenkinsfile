pipeline {
    /* A Declrative Pipeline */
    agent any /*Any agent can run. Master or Slave*/

    stages{ 
        stage('Build'){
            steps{
                bat 'mvn clean package'
            }
            post{
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging'){
            steps{
                build job: 'deploy-to-staging'
            }
        }
        stage('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    inout message: 'Approve PRODUCTION Deployment?'
                }
                build job: 'deploy-to-production'
            }
            post{
                success{
                    echo 'Code deployed to Production.'
                }
                failure{
                    echo 'Deployment failed.'
                }
            }
        }
    }
}
