pipeline {
    agent any
    environment {
          APP_NAME = "reddit-clone-pipeline"
    }
    stages {
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     git branch: 'main', credentialsId: 'github', url: 'https://github.com/Shubham4676/a-reddit-clone-gitops'
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
         stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "Shubham4676"
                    git config --global user.email "meghraj4467@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/Shubham4676/a-reddit-clone-gitops main"
                }
            }
         }
    }
}
