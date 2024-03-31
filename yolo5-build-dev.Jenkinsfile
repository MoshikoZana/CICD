pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('docker_credentials')
        IMAGE_URL = "moshikozana/cicd-yolo-dev"
    }
    stages {
        stage('Build') {
            steps {
                // Navigate to the directory containing Dockerfile
                dir('yolo5') {
                    sh '''
                        pwd
                        echo \${BUILD_NUMBER}
                        docker build -t yolocicd:dev.${BUILD_NUMBER} .
                        docker tag yolocicd:dev.${BUILD_NUMBER} $IMAGE_URL:dev.${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Upload image to Docker Hub') {
            steps {
                sh'''
                    echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin
                    docker push $IMAGE_URL:dev.${BUILD_NUMBER}
                '''
            }
            post {
                always {
                    script {
                        sh '''
                            docker system prune -a --force
                        '''
                    }
                }
            }
        }

        stage('Trigger Deploy job') {
            steps {
                build job: 'yolo5-dev-releases', wait: false, parameters: [
                    string(name: 'YOLO5_DEV_IMAGE_URL', value: "${IMAGE_URL}:dev.${BUILD_NUMBER}")
                ]
            }
        }
    }
}
