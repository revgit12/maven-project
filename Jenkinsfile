pipeline{
	agent any
	parameters{
	string(name: 'tcat', defaultValue: '172.31.11.167', description: 'Tomcat Server')
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
					archiveArtifacts artifacts:'**/target/*.war'
				}
			}
		}
		stage('Deployment'){
			parallel{
				stage('Staging Deployment'){
					steps{
						sh "scp -P 443 webapp/target/*.war root@${params.tcat}:/root/tomcat/tcat-stg/webapps/"
				    }
				}
				stage('Production Deployment'){
					steps{
						sh "scp -P 443 webapp/target/*.war root@${params.tcat}:/root/tomcat/tomcat/webapps/"
				    }
				}
			}
		}
	}
}
