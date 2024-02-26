pipeline {
    agent {
        label "slave"
    }
    environment {
        APP_NAME = "registerapp"
    }
    stages {
        stage ('cleanws') {
            steps {
                cleanWs()
            }
        }
        stage ('checkout') {
            steps {
                git branch: 'master', credentialsId: 'gitcred', url: 'https://github.com/vigneshrepo23/register-app-gitops'
            }
        }
         stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${DOCKER_USER}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "vigneshrepo23"
                   git config --global user.email "vigneshbabu2394@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'gitcred', gitToolName: 'Default')]) {
                  sh "git push https://github.com/vigneshrepo23/register-app-gitops master"
                }
            }
        }
    }    
}