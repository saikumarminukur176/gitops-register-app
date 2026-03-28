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
                                                                        git branch: 'main', credentialsId: 'github-pat', url: 'https://github.com/saikumarminukur176/gitops-register-app.git'
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
                                                                        withCredentials([usernamePassword(credentialsId: 'github-pat', passwordVariable: 'GIT_PAT', usernameVariable: 'GIT_USER')]) {
                                                                                                    sh "git push https://${GIT_USER}:${GIT_PAT}@github.com/saikumarminukur176/gitops-register-app.git main"
                                                                        }
                                                }
                            }
            }
}
