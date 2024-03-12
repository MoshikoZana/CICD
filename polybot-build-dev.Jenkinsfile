pipeline {
    agent any

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
                    // Generating unique image tag
                    def imageTag = "${env.BUILD_NUMBER}-${env.GIT_COMMIT}"

                    // Build Docker image
                    script {
                        docker.build("moshikozana/cicd-poly:${imageTag}")
                    }
                }
            }
        }
        stage('Push') {
            steps {
                // Push Docker image to Docker registry
                script {
                    docker.withRegistry('https://hub.docker.com/', 'docker_credentials') {
                        docker.image("moshikozana/cicd-poly:${imageTag}").push()
                    }
                }
            }
        }
    }
}