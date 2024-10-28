pipeline {
    agent any
    stages {
        stage('Build and Test') {
            steps {
                script {
                    def workingDir = isUnix() ? "${env.WORKSPACE}" : "/c${env.WORKSPACE.substring(2).replace('\\', '/')}"
                    docker.image('cypress/browsers').inside {
                        dir(workingDir) {
                            sh 'mvn clean install'
                            sh 'npx cypress run'
                        }
                    }
                }
            }
        }
    }
}

