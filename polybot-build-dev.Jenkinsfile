pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker_credentials')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from repository
                git branch: 'dev', url: 'https://github.com/MoshikoZana/CICD.git'
            }
        }
        stage('Build') {
            steps {
                // Navigate to polybot directory
                dir('polybot') {
                    // Generate a unique image tag
                    script {
                        def imageTag = "${env.BUILD_NUMBER}-${env.GIT_COMMIT}"

                        // Build Docker image
                        docker.build("moshikozana/cicd-poly:${imageTag}")
                    }
                }
            }
        }
        stage('Login and Push to Dockerhub') {
            steps {
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Login to Docker Hub
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"

                    // Push Docker image to Docker registry
                    docker.withRegistry('https://hub.docker.com/', 'docker_credentials') {
                        docker.image("moshikozana/cicd-poly:${imageTag}").push()
                    }
                }
            }
        }
    }
}
