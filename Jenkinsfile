
pipeline {
    /*declaring pipeline */

    agent any /*agent can define here,this case any agent */

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }



        stage ('SonarQube analysis'){
            def mvnHome = tool name: 'maven-3', type: 'maven'
            withSonarQubeEnv('server-sonar') {
                sh "${mvnHome}/bin/mvn sonar:sonar"
            }
            
        }
        stage ('Email Notification'){
            mail bcc: '',body: '''Hi welcome to jenkins email alerts
            Thanks
            Hari''', cc: '',from: '',replyTo:'', subject:'jenkins job' to:dammithar@gmil.com
        }
            
        
        stage ('Deploy to Staging'){
            steps {
                build job: 'deploy_to_staging'
            }
        }

        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }


    }
}
