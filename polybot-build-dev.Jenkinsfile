pipeline {
    agent any

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
            environment {
                DOCKER_USERNAME = credentials('docker_username')
                DOCKER_PASSWORD = credentials('docker_password')
            }
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker_credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        docker.withRegistry('https://hub.docker.com/', 'docker_credentials') {
                            docker.image("moshikozana/cicd-poly:${imageTag}").push()
                        }
                    }
                }
            }
        }
    }
}
