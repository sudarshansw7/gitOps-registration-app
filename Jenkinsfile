pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "registration-app-cd"
              GITHUB_TOKEN = credentials('github-token')
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'master', credentialsId: 'github-token', url: 'https://github.com/sudarshansw7/gitOps-registration-app'
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
                   git config --global user.name "sudarshansw7"
                   git config --global user.email "sudarshansw7@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-token', gitToolName: 'Default')]) {
                  sh "git push https://github.com/sudarshansw7/gitOps-registration-app master"
                 }
            }
        }
      
    }
}
