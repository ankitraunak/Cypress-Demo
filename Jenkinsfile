pipeline {
    agent {
        docker {
            image 'cypress/browsers'    // Use Cypress's Docker image with browsers
            args '-u root'              // Run as root to handle permissions
        }
    }
    environment {
        CYPRESS_BASE_URL = 'https://www.saucedemo.com'
    }
    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                sh 'npm ci'                  // Install dependencies from package-lock.json
                sh 'npm install mochawesome' // Install mochawesome for reporting
            }
        }

        stage('Run Cypress Tests') {
            steps {
                echo 'Running Cypress tests...'
                sh 'npx cypress run --reporter mochawesome' // Run tests with mochawesome reporting
            }
            post {
                always {
                    // Archive test artifacts
                    archiveArtifacts artifacts: 'mochawesome-report/**/*', allowEmptyArchive: true
                    archiveArtifacts artifacts: 'cypress/screenshots/**/*.png', allowEmptyArchive: true
                    archiveArtifacts artifacts: 'cypress/videos/**/*.mp4', allowEmptyArchive: true
                }
                success {
                    echo 'Cypress tests completed successfully.'
                }
                failure {
                    echo 'Tests failed. Check the artifacts for more information.'
                }
            }
        }
    }

    post {
        cleanup {
            // Clean up resources
            echo 'Performing cleanup...'
            sh 'docker system prune -f'
        }
    }
}
