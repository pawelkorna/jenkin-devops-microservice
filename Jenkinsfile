//scripted 
// node {
	
// 	echo "Build"
// 	echo "Test"
// 	echo "Test"
// }

//declarative pipeline
pipeline {
	agent any
	//agent { docker { image 'maven:3.6.3'} }
	// agent { docker { image 'node:alpine3.16'} }
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
		stages {
		stage('Checkout') {
			steps {
				sh "mvn --version"
				sh "docker version"
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"
				echo "JOB_NAME - $env.JOB_NAME"

			}
		}

		stage ('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}
		stage('Test') {
			steps {
				sh "mvn test"
			}
		}
		stage('Integration Test') {
			steps {
				sh "mvn failsafe:integration-test failsafe:verify"
			}
			
		}
		stage('PAckage') {
			steps {
				sh "mvn package -DskipTests"
			}
		}

		stage('Build Docker image') {
			steps {
				//"docker build -t pawelkornasiewicz/currency-exchange-devops:$env.BUILD_TAG ."
				script {
					dockerImage = docker.build("pawelkornasiewicz/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
			
		}
		stage('Pushing image to the dockerHub') {
			steps {
				script {
					docker.withRegistry('', 'dockerhub') {
					dockerImage.push();
					dockerImage.push('latest');
					}
	
				}	

			}
			
		}
	} 
	post {
		always {
			echo 'Im awesome. I run always'
		}
		success {
		echo ' I run when the build success'
		}
		failure {
		echo ' I run when the build fails'
		}
		changed {
		echo 'i run when there was change from failed to success or otherwise'		
		}
	}

}

