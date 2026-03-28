pipeline {
    agent { label "Jenkins-Agent" }

    environment {
        APP_NAME = "ashfaque9x/register-app-pipeline"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    tools {
        jdk "java-11"
        maven "maven-3.8.5"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                // CHANGED: Now looking for the ID 'saikumarminukur176'
                git branch: 'main', credentialsId: 'saikumarminukur176', url: 'https://github.com/saikumarminukur176/gitops-register-app.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's|image: ${APP_NAME}:.*|image: ${APP_NAME}:${IMAGE_TAG}|g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "saikumarminukur176"
                    git config --global user.email "saikumarminukuri@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest to tag ${IMAGE_TAG}"
                """
                
                // CHANGED: Now looking for the ID 'saikumarminukur176'
                withCredentials([gitUsernamePassword(credentialsId: 'saikumarminukur176', gitToolName: 'Default')]) {
                    sh "git push https://github.com/saikumarminukur176/gitops-register-app.git main"
                }
            }
        }
    }
}
