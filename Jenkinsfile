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
                    sed -i 's/reddit-clone-app.*/reddit-clone-app-1-0-0-12/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "zylmaimun"
                    git config --global user.email "simeon.k.sabev@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/zylmaimun/reddit-clone-gitops main"
                }
            }
        }
    }
}
