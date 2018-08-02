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
  }
}
