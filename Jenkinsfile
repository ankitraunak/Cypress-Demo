pipeline {
    agent {
        docker {
            image 'cypress/browsers'  // Use Cypress's Docker image with browsers for testing
            args '-u root -w //d/ProgramData/Jenkins/.jenkins/workspace/CyprssTestPipeline/' // Corrected path
        }
    }

    environment {
        CYPRESS_BASE_URL = 'https://www.saucedemo.com' // Base URL for Cypress tests
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm // Checkout code from SCM
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    sh 'npm ci'
                    sh 'npm install mochawesome'
                }
            }
        }

        stage('Run Cypress Tests') {
            steps {
                script {
                    echo 'Running Cypress tests...'
                    sh 'npx cypress run --reporter mochawesome'
                }
            }
            post {
                always {
                    archiveArtifacts artifacts: 'mochawesome-report/**/*', allowEmptyArchive: true
                    archiveArtifacts artifacts: 'cypress/screenshots/**/*.png', allowEmptyArchive: true
                    archiveArtifacts artifacts: 'cypress/videos/**/*.mp4', allowEmptyArchive: true
                }
                success {
                    echo 'Tests passed!'
                }
                failure {
                    echo 'Tests failed.'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }
    }
}
