//scripted 
// node {
	
// 	echo "Build"
// 	echo "Test"
// 	echo "Test"
// }

//declarative pipeline
pipeline {
	//agent any
	//agent { docker { image 'maven:3.6.3'} }
	agent { docker { image 'node:alpine3.16'} }
		stages {
		stage('Build') {
			steps {
				//sh "mvn --version"
				sh "node --version"
				echo "Build"
			}
		}
		stage('Test') {
			steps {
				echo "Test"
			}
		}
		stage('Integration Test') {
			steps {
				echo "Integration Test"
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

