pipeline {
    agent { label "Jenkins-Agent" }

    environment {
        APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', url: 'https://github.com/rajnages/gitops-register-app.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')]) {
                    sh """
                       git config --global user.name "rajnages"
                       git config --global user.email "nagesrajavarapu@gmail.com"
                       git remote set-url origin https://rajnages:${GITHUB_TOKEN}@github.com/rajnages/gitops-register-app.git
                       git add deployment.yaml
                       git commit -m "Update deployment tag to ${IMAGE_TAG} for ${APP_NAME}"
                       git push origin main
                    """
                }
            }
        }
    }

    post {
        failure {
            echo 'The pipeline failed. Investigate the logs.'
        }
        success {
            echo 'The pipeline executed successfully! Deployment tags updated.'
        }
    }
}
