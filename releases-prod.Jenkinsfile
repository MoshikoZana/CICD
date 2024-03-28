pipeline {
    agent any

    parameters {
        string(name: 'POLYBOT_PROD_IMAGE_URL', defaultValue: '', description: 'URL for Polybot production image')
        string(name: 'YOLO5_PROD_IMAGE_URL', defaultValue: '', description: 'URL for YOLO5 production image')
    }

    stages {
        stage('Update YAML') {
            steps {
                script {
                    // Check if POLYBOT_PROD_IMAGE_URL is provided
                    if (env.POLYBOT_PROD_IMAGE_URL && env.POLYBOT_PROD_IMAGE_URL.trim()) {
                        // Run commands related to Polybot
                        polybotSteps()
                    } else if (env.YOLO5_PROD_IMAGE_URL && env.YOLO5_PROD_IMAGE_URL.trim()) {
                        // Run commands related to YOLO5
                        yolo5Steps()
                    } else {
                        error 'No production image URL provided. Aborting.'
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

def polybotSteps() {
    // Polybot specific steps
    withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh """
        git config --global user.email "Jenkins@ip-10-0-0-178"
        git config --global user.name "Jenkins"
        git checkout releases
        git branch

        sed -i "s|image: .*|image: ${POLYBOT_PROD_IMAGE_URL}|g" k8s/prod/Polybot.yaml

        cat k8s/prod/Polybot.yaml
        git add k8s/prod/Polybot.yaml
        git commit -m "${POLYBOT_PROD_IMAGE_URL}"
        git push https://moshikozana:${PASSWORD}@github.com/MoshikoZana/Object-Detection-Service-CICD.git releases
        """
    }
}

def yolo5Steps() {
    // YOLO5 specific steps
    withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh """
        git config --global user.email "Jenkins@ip-10-0-0-178"
        git config --global user.name "Jenkins"
        git checkout releases
        git branch
        sed -i "s|image: .*|image: ${YOLO5_PROD_IMAGE_URL}|g" k8s/prod/yolo5.yaml
        cat k8s/prod/yolo5.yaml
        git add k8s/prod/yolo5.yaml
        git commit -m "${YOLO5_PROD_IMAGE_URL}"
        git push https://moshikozana:${PASSWORD}@github.com/MoshikoZana/Object-Detection-Service-CICD.git releases
        """
    }
}
