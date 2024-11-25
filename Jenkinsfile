pipeline {
	agent any
	environment {
		GIT_REPOSITORY_URL = "https://github.com/SanketCIGIT/docker_jenkins.git"
		DOCKER_IMAGE_NAME = 'sanketcigit/docker_jenkins'
		IMAGE_TAG = '1.0' 
	}
	stages {
		stage('Clone Repository') {
			steps {
				script {
					try {
						git branch: 'main', url: 'https://github.com/SanketCIGIT/docker_jenkins.git'
					} catch (Exception e) {
						echo "Failed to clone repo : ${e.message}"
						error "Failed to clone repo"
						}
				}
			}
		}	
		stage('Build') {
			steps {
				script {
					try {
						docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")	
					} catch (Exception e) {
						echo "Failed to build docker image : ${e.message}"
						error "Failed to build docker image"
						}
				}
			}
		}	
		stage('Push to dockerhub') {
			steps {
				script {
					try {
						withCredentials([usernamePassword(credentialsId: 'ditiss', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
						// Explicit login before push
						sh """
						echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
						docker push ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}
						"""
							} 
					} catch (Exception e) {
						echo "Failed to push docker image : ${e.message}"
						error "Failed to push docker image"
						}
				}
			}
		}
	}		
}	
