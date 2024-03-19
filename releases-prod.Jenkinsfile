pipeline {
    agent {
        image ''
        args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
    }
    environment {
        DOCKER_CREDENTIALS = credentials('docker_credentials')
        IMAGE_URL = "moshikozana/cicd-poly"
        DOCKER_IMAGE_POLYBOT = ""
    }
     stages {
        stage('Update YAML Manifests') {
            steps {
                script {
                    // Pull the latest Docker images from Dockerhub
                    docker.withRegistry('https://index.docker.io/v1/', 'docker_credentials') {
                        docker.image(DOCKER_IMAGE_POLYBOT).pull()