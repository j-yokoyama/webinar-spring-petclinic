pipeline {
	agent {
		docker { image 'maven:3.6-jdk-11-slim' }
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
