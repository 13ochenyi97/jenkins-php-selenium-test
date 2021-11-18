pipeline {
	agent none
	stages {
		stage('Integration UI Test') {
			parallel {
				stage('Deploy') {
					agent any
					steps {
						sh 'chmod 777 ./jenkins/scripts/deploy.sh'
						sh 'chmod 777 ./jenkins/scripts/kill.sh'
						sh 'sed $'s/\r$//' ./jenkins/scripts/deploy.sh > ./jenkins/scripts/deploy.Unix.sh'
						sh 'sed $'s/\r$//' ./jenkins/scripts/kill.sh > ./jenkins/scripts/kill.Unix.sh'
						sh './jenkins/scripts/deploy.Unix.sh'
						input message: 'Finished using the web site? (Click "Proceed" to continue)'
						sh './jenkins/scripts/kill.Unix.sh'
					}
				}
				stage('Headless Browser Test') {
					agent {
						docker {
							image 'maven:3-alpine' 
							args '-v /root/.m2:/root/.m2' 
						}
					}
					steps {
						sh 'mvn -B -DskipTests clean package'
						sh 'mvn test'
					}
					post {
						always {
							junit 'target/surefire-reports/*.xml'
						}
					}
				}
			}
		}
	}
}
