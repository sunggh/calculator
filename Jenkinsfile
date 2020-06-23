pipeline {

agent any
environment {
	registry = "gch03944/calculator"
	registryCredential="dockerhub"
	dockerImage=''
}

stages {
	stage ('Chekc out'){
		steps {
			git branch : 'uat', url: 'https://github.com/juht/calculator.git'
		}
	}
	stage ('Compile') {
		steps {
			sh './gradlew compileJava'
		}
	}
	stage ('Unit Test') {
		steps {
			sh "./gradlew test"
		}
	}
	stage ('Code Coverage'){
		steps {
			sh "./gradlew jacocoTestReport"
			sh "./gradlew jacocoTestCoverageVerification"
		}
	}
	stage ('Static Code Analysis'){
		steps {
			sh "./gradlew checkstyleMain"
			publishHTML (target: [
			reportDir: 'build/reports/checkstyle/',
			reportFiles: 'main.html',
			reportName: "Checkstyle Report"
			])
		}
	}
	stage ('Packaging'){
		steps {
			sh './gradlew build'
		}

	}
	stage ('Deploy Image'){
		steps {
			script {
				dockerImage = docker.build registry + ":$BUILD_NUMBER"
			}
		}
	}



}
}
