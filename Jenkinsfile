pipeline {
        agent { label "Jenkins-Agent" }

        // 1. ADD THIS BLOCK so sed knows what to replace
        environment {
                    APP_NAME = "ashfaque9x/register-app-pipeline" // Change this to match your actual app name
                    IMAGE_TAG = "${env.BUILD_NUMBER}" // Or whatever dynamic tag you use
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
                                                              git branch: 'main', credentialsId: 'github-pat', url: 'https://github.com/saikumarminukur176/gitops-register-app.git'
                                       }
                    }

                    stage("Update the Deployment Tags") {
                                    steps {
                                                        // 2. sed will now successfully use the variables defined above
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
                                                                           git config --global user.name "saikumarminuku176"
                                                                                              git config --global user.email "saikumarminukuri@gmail.com"
                                                                                                                 git add deployment.yaml
                                                                                                                                    git commit -m "Updated Deployment Manifest to tag ${IMAGE_TAG}"
                                                                                                                                                    """

                                                        // 3. Bulletproof Git Push (Injects token directly into URL securely)
                                                        withCredentials([usernamePassword(credentialsId: 'github-pat', passwordVariable: 'GIT_PAT', usernameVariable: 'GIT_USER')]) {
                                                                              sh "git push https://${GIT_USER}:${GIT_PAT}@github.com/saikumarminukur176/gitops-register-app.git main"
                                                        }
                                    }
                    }
        }
}
