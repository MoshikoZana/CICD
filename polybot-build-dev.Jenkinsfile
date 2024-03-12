pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker_credentials')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/MoshikoZana/CICD.git'
            }
        }
        stage('Build') {
            steps {
                dir('polybot') {
                    script {
                        def imageTag = "${env.BUILD_NUMBER}-${env.GIT_COMMIT}"
                        docker.build("moshikozana/cicd-poly:${imageTag}")
                    }
                }
            }
        }
        stage('Login and Push to Dockerhub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        docker.withRegistry(url: 'https://hub.docker.com/', credentialsId: 'docker_credentials') {
                            docker.image("moshikozana/cicd-poly:${imageTag}").push()
                        }
                    }
                }
            }
        }
    }
}
