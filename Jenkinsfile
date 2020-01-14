pipeline {
    environment {
        mvn_opts = "-Dmaven.repo.local=/tmp/.m2/repository"
    }

	agent any

	options {
		timestamps()
	}

	stages {
	    
		stage('Prepare') {
			agent {
                docker { 
                    image 'maven:3.6-jdk-11-slim'
                    args '-v /var/lib/jenkins/.m2:/tmp/.m2'
                }
            }
			steps {
				sh 'mvn ${mvn_opts} clean'
			}
		}

		stage('Unit Test') {
			agent {
                docker { 
                    image 'maven:3.6-jdk-11-slim'
                    args '-v /var/lib/jenkins/.m2:/tmp/.m2'
                }
            }
			steps {
				sh 'mvn ${mvn_opts} test -Dmaven.test.failure.ignore=true'
			}
		}

		stage('Build App') {
			agent {
                docker { 
                    image 'maven:3.6-jdk-11-slim'
                    args '-v /var/lib/jenkins/.m2:/tmp/.m2'
                }
            }
			steps {
				sh 'mvn ${mvn_opts} package -Dmaven.test.skip=true'
			}
		}

		stage('Build Docker Image') {
			steps {
				sh '''
				    docker build -t petclinic:latest -f docker/Dockerfile .
				'''
			}
		}
		
		stage('Push Docker Image') {
            steps{
                script {
                    docker.withRegistry( 'https://webinarsample.azurecr.io', 'acr_webiner_sample' ) {
                        docker.image('petclinic:latest').push('latest')
                    }
                }
            }
		}
		
		stage('Deploy Webb App') {
            steps{
                azureWebAppPublish appName: 'petclinic', azureCredentialsId: 'az-service-principal', dockerImageName: 'petclinic', dockerImageTag: 'latest', dockerRegistryEndpoint: [ url: 'https://webinarsample.azurecr.io', credentialsId: 'acr_webiner_sample' ], filePath: '', publishType: 'file', resourceGroup: 'Webinar', slotName: '', sourceDirectory: '', targetDirectory: ''
            }
		}
	}
}
