pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/your-image-name"
        DOCKER_CREDENTIALS = "dockerhub-credentials"  // This is the ID of your Docker Hub credentials
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub using Jenkins credentials
                    docker.withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    sh 'docker push ${DOCKER_IMAGE}:${env.BUILD_ID}'
                }
            }
        }
    }

    post {
        always {
            // Clean up by logging out of Docker Hub
            sh 'docker logout'
        }
    }
}
