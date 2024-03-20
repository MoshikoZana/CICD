pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('docker_credentials')
        IMAGE_URL = "moshikozana/cicd-poly"
    }
    stages {
        stage('Build') {
            steps {
                // Navigate to the directory containing Dockerfile
                dir('polybot') {
                    sh '''
                        pwd
                        echo ${BUILD_NUMBER}
                        docker build -t polybotcicd:${BUILD_NUMBER} .
                        docker tag polybotcicd:${BUILD_NUMBER} $IMAGE_URL:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Upload image to Docker Hub') {
            steps {
                sh'''
                    echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin
                    docker push $IMAGE_URL:${BUILD_NUMBER}
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
    }
}

       stage('Trigger Deploy job') {
            steps {
                 build job: 'releases', wait: false, parameters: [
                        string(name: 'POLYBOT_PROD_IMAGE_URL', value: "${IMAGE_URL}:${BUILD_NUMBER}.prod")
                    ]
                    if (deploy_job == "FAILURE") {
                        error "Deploy job failed"
                    }
                }
            }
        }
    }
}