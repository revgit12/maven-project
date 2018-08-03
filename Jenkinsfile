pipeline {
  agent any
  stages {
      stage('Build'){

        steps{
            sh 'mvn clean package'
          }
        post {
            success {
               echo 'Now Archiving...'
	       archiveArtifacts artifacts: '**/*.war'
            }
          }
        }
      stage('Deploy to Staging'){
        steps{
            build job:'j3-deploy2stg'
          }
        }
      stage('Deploy to Production'){
        steps{
          timeout(time:4,unit:'DAYS'){
             input message:'Provide Approval'
             }
          build job:'j5-deploy_prod'
        }
        post{
            success{
               echo 'Deployment Successful'
               }
            failure{
               echo 'Deployment Failed'
               }
            }
          
        }
  }
}
