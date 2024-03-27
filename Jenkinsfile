def frontendImage="markre94/frontend-custom"
def backendImage="markre94/backend-custom"
def dockerRegistry=""
def registryCredentials="dockerhub"
def backendDockerTag=""
def frontendDockerTag=""

pipeline {
    agent {
        label 'agent'
    }

    parameters {
        string defaultValue: '""', name: 'backendDockerTag'
        string defaultValue: '""', name: 'frontendDockerTag'
    }


    stages {
        stage('Get code') {
            steps {
                checkout scm
            }
        }

        stage('Adjust version') {
            steps {
                script{
                    backendDockerTag = params.backendDockerTag.isEmpty() ? "latest" : params.backendDockerTag
                    frontendDockerTag = params.frontendDockerTag.isEmpty() ? "latest" : params.frontendDockerTag
                   
                    currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}"
                }
            }
    }
}
}
