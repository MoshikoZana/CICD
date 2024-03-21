pipeline {
    agent any

    parameters {
        string(name: 'POLYBOT_PROD_IMAGE_URL', defaultValue: '', description: '')
    }
    stages {
        stage('Update YAML') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                    sh """
                    git config --global user.email "Jenkins@ip-10-0-0-178"
                    git config --global user.name "Jenkins"
                    git checkout releases
                    git merge origin/main

                    sed -i 's|image: .*|image: ${POLYBOT_PROD_IMAGE_URL}|g' k8s/prod/Polybot.yaml

                    cat k8s/prod/Polybot.yaml
                    git add k8s/prod/Polybot.yaml
                    git commit -m "$POLYBOT_PROD_IMAGE_URL"
                    git push https://moshikozana:$PASSWORD@github.com/MoshikoZana/Object-Detection-Service-CICD.git releases
                    """
                }
            }
        }
    }
}