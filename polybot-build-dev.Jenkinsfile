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
        stage('Push') {
            steps {
                // Push Docker image to Docker registry
                script {
                    docker.withRegistry('https://hub.docker.com/', '$DOCKER_CREDENTIALS') {
                        docker.image("moshikozana/cicd-poly:${imageTag}").push()
                    }
                }
            }
        }
    }
}
