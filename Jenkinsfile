pipeline {
	agent {
		docker { image 'maven:3.6-jdk-slim' }
	}

	options {
		timestamps()
	}

	stages {
		stage('Prepare') {
			steps {
				sh 'mvn --version'
			}
		}
	}
}
