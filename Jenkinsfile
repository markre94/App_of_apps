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
        string 'backendDockerTag'
        string 'frontendDockerTag'
    }

    environments {
        PIP_BREAK_SYSTEM_PACAKGES=1
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

        stage('Remove containers') {
            steps {
                sh 'docker rm -f backend'
                sh 'docker rm -f frontend'
            }
        }

        stage('Deploy application') {
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                       docker.withRegistry("$dockerRegistry", "$registryCredentials") {
                            sh "docker-compose up -d"
                }
                             }
                }
            }
        }

        stage('Selenium tests') {
            steps {
                sh 'pip3 install -r ./test/selenium/requirements.txt'
                sh 'python3 -m pytest ./test/selenium/frontendTest.py'
            }

        }
    
    }

    post {
        always {
            sh 'docker-compose down'
            cleanWs()
        }
    }

}
