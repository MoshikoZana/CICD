pipeline {
    agent any
    environment{
        DOCKER_CREDENTIALS = credentials('docker_credentials')
        IMAGE_URL = "moshikozana/cicd-poly"
    }
    stages {
        //sh def buildNumber=${env.BUILD_NUMBER}
        stage('Build') {
            steps {
            sh '''
                pwd
                echo ${BUILD_NUMBER}
                docker build -t polybotcicd:${BUILD_NUMBER} .
                docker tag polybotcicd:${BUILD_NUMBER} $IMAGE_URL:${BUILD_NUMBER}
            '''
            }
        }
        stage('upload image to dockerhub:') {
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
        stage('Trigger Deploy job') {
            steps {
                string deploy_job=build job: 'polybotDeploy', wait: false, parameters: [
                    string(name: 'POLY_IMAGE_URL', value: "${IMAGE_URL}:${BUILD_NUMBER}")
                ]
                sh '''
                    if [[ deploy_job == "FAILURE" ]]
                    then {
                        exit 1
                    }
                    fi
                '''
            }
        }
    }
}