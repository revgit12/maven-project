pipeline{
	agent any
	parameters{
	string(name: 'tcat',defaultValue:'13.127.20.108',description:'Tomcat Server')
	}
	triggers{
	pollSCM('* * * * *')
	}
	stages{
		stage('Build'){
			steps{
				sh 'mvn clean package'
			}
			post{
				success{
					echo 'Archiving artifacts'
					archiveArtifacts artifacts:'**/*.war'
				}
			}
		}
		stage('Deployment'){
			parallel{
				stage('Staging Deployment'){
					steps{
						sh 'scp **/target/*.war root@${params.tcat}:/root/tomcat/tcat-stg/webapps/.'
				    }
				}
				stage('Production Deployment'){
					steps{
						sh 'scp **/target/*.war root@${params.tcat}:/root/tomcat/tomcat/webapps/.'
				    }
				}
			}
		}
	}
}
