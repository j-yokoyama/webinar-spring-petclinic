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
			archiveArtifacts 'target/*.jar'
		}

		stage('Build Docker Image') {
			steps {
				sh '''
				    mkdir -p ./target
				    cp -fp /var/lib/jenkins/workspace/Sample/spring-petclinic@2/target/*.jar ./target
				    docker build -t petclinic:latest -f docker/Dockerfile .
				'''
			}
		}
		
		stage('Push Docker Image') {
            steps{
                script {
                    docker.withRegistry( 'https://webinarsample.azurecr.io', 'acr_webiner_sample' ) {
                        docker.image('petclinic').push('latest')
                    }
                }
            }
		}
		
		// stage('Deploy Webb App') {
        //     steps{
        //         azureWebAppPublish appName: 'petclinic', azureCredentialsId: 'az-service-principal', dockerImageName: 'petclinic', dockerImageTag: 'latest', dockerRegistryEndpoint: [ url: 'https://webinarsample.azurecr.io', credentialsId: 'acr_webiner_sample' ], publishType: 'docker', resourceGroup: 'Webinar'
        //     }
		// }
	}
}
