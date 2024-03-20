pipeline {
    agent any

    parameters {
        string(name: 'POLY_IMAGE_URL', defaultValue: '', description: '')
    }

    stages {
        stage('Update YAML') {
            steps {
                sh """
                git checkout releases
                git merge main

                sed -i 's/image: .*/image: $POLY_IMAGE_URL/g' k8s/prod/polybot.yaml

                git add k8s/prod/polybot.yaml
                git commit -m "$POLY_IMAGE_URL"
                git push origin releases
                """
            }
        }
    }
}