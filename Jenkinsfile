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
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/rajnages/gitops-register-app.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "rajnages"
                   git config --global user.email "nagesrajavarapu@gmail.com"
                   git config --global credential.https://github.com/rajnages/gitops-register-app.git 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
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                   git push https://rajnages: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@github.com/rajnages/gitops-register-app main
                """
                // withCredentials([string(credentialsId: 'token1', variable: 'token1', gitToolName: 'Default')]){
                //   sh "git push https://github.com/sagarkulkarni1989/gitops-register-app main"
                // }
            }
        }
      
    }
}
