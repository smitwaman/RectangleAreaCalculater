pipeline {
    agent any
     tools {
        
  jdk 'jdk'
  maven 'maven'
  dockerTool 'docker'
  
    }
    environment {
        DOCKER_HUB_REGISTRY = 'docker.io' // Docker Hub registry URL
        DOCKER_HUB_USERNAME = 'smitwaman' // Your Docker Hub username
        DOCKER_HUB_REPOSITORY = 'webapp' // Your Docker Hub repository name
        DOCKER_IMAGE_TAG = 'env.BUILD_TAG' // Tag for the Docker image
        DOCKER_IMAGE = 'javaapp'
       }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

    stages {
        stage('Build and Test') {
            steps {
                // Checkout the code from your repository
                git 'https://github.com/smitwaman/rectangle-area-calculator.git'

                // Build and test the project using Maven
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image with Dockerfile
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}", "-f Dockerfile .")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                // Log in to Docker Hub
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                            // Push Docker image to Docker Hub
                            docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
                        }
                    }
                }
            }
        }
    }
}
