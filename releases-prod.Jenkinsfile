pipeline {
    agent any

    parameters {
        string(name: 'POLY_IMAGE_URL', defaultValue: '', description: '')
    }

    stages {
        stage('Update YAML') {
            steps {
                sh """
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                git config --global user.email "Jenkins@ip-10-0-0-178"
                git config --global user.name "Jenkins"
                git checkout releases
                git merge main

                sed -i "s|image: .*|image: $POLY_IMAGE_URL|g" k8s/prod/polybot.yaml

                git add k8s/prod/polybot.yaml
                git commit -m "$POLY_IMAGE_URL"
                git push https://moshikozana:$PASSWORD@github.com/MoshikoZana/Object-Detection-Service-CICD.git releases
                """
            }
        }
    }
}