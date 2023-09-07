pipeline {
	agent any
	tools {        // access build tools
		maven 'maven-3.9.4'
	}	
	stages {
		stage("build jar") {
			steps {
				script {
					echo "building the application..."
					sh 'mvn package'
				}
			}
		}
		stage("build image") {
			steps {
				script {
					echo "building the application..."
					withCredentials([usernamePassword(credentialsID: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')])
						sh 'docker build -t nanatwn/demo-app:jma-2.0'
						sh 'echo $PASS | docker login -u $USER --password-stdin'
						sh 'docker push ryan02/demo-app:jma-2.0'
				}
			}
		}
		stage("deploy") {
			steps {
				script {
					echo "deploying the application..."
				}
			}
		}
	}
}
