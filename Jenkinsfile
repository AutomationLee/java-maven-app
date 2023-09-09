pipeline {     // testing for webhooks trigger
	agent any
	tools {        // access build tools
		maven 'maven-3.9.4'
	}	
	stages {
		stage("increment version") {
			steps {
				script {
					echo 'incrementing app version...'
					sh 'mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.nextMinorVersion}.\\\${parsedVersion.incrementalVersion} versions:commit'
					def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
					def version = matcher[0][1]     // index 0 is the version and 1 is the number in pom.xml
					env.IMAGE_NAME = "$version-$BUILD_NUMBER"
				}
			}
		}
		stage("build app") {
			steps {
				script {
					echo "building the application..."
					sh 'mvn clean'
				}
			}
		}
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
					withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
						sh "docker build -t ryan02/demo-app:$IMAGE_NAME ."
						sh 'echo $PASS | docker login -u $USER --password-stdin'
						sh "docker push ryan02/demo-app:${IMAGE_NAME}"
				       }
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
        stage('commit version update'){
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'LeeAutomation', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh 'git status'
                        sh 'git branch'
                        sh 'git config --list'

                        sh "git remote set-url origin https://${USER}:${PASS}@github.com/AutomationLee/java-maven-app.git"
                        sh 'git add .'
                        sh 'git commit -m "pom updated"'
                        sh 'git push origin HEAD:main'
                    }
                }
            }
	}
	}
}
