pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from repository
                git 'https://github.com/yolo5'
            }
        }
        stage('Build') {
            steps {
                // Build Docker image
                script {
                    docker.build('moshikozana/yolo-k8s:dev')
                }
            }
        }
        stage('Push') {
            steps {
                // Push Docker image to Docker registry
                script {
                    docker.withRegistry('https://your-docker-registry', 'docker-credentials-id') {
                        docker.image('moshikozana/yolo-k8s:dev').push()
                    }
                }
            }
        }
    }
}
