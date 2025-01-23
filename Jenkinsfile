pipeline {
    agent any
    environment {
        APP_NAME = "reddit-clone-app"
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/zylmaimun/reddit-clone-gitops'
            }
        }
        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}-${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }
        stage("Update the Service Tags") {
            steps {
                sh """
                    cat service.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}-${IMAGE_TAG}/g' service.yaml
                    cat service.yaml
                """
            }
        }
        stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "zylmaimun"
                    git config --global user.email "simeon.k.sabev@gmail.com"
                    git add deployment.yaml service.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/zylmaimun/reddit-clone-gitops main"
                }
            }
        }
    }
}
