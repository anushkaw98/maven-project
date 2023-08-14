
pipeline {
    /*declaring pipeline */

    agent any /*agent can define here,this case any agent */

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving ...'
                    archiveArtifacts artifacts: '**/target/*.war'             
                }
            }    
        }

        stage ('Deploy to Staging') {
            steps {
                build job: 'pipeline-Deploy-to-staging'

            }
        }




    }
}