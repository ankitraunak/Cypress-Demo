pipeline {
    agent {
        docker {
            image 'cypress/browsers'  // Use the Cypress image with necessary browsers
            args '-u root'            // Run as root for permissions management
        }
    }

    environment {
        // Set environment variables if needed
        CYPRESS_BASE_URL = 'https://www.saucedemo.com'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                // Install npm packages and mochawesome for reporting
                echo 'Installing dependencies...'
                sh 'npm ci'
                sh 'npm install mochawesome'
            }
        }

        stage('Run Cypress Tests') {
            steps {
                // Run Cypress with the mochawesome reporter
                echo 'Running Cypress tests...'
                sh 'npx cypress run --reporter mochawesome'
            }
            post {
                always {
                    // Archive test artifacts, screenshots, and videos
                    archiveArtifacts artifacts: 'mochawesome-report/**/*', allowEmptyArchive: true
                    archiveArtifacts artifacts: 'cypress/screenshots/**/*.png', allowEmptyArchive: true
                    archiveArtifacts artifacts: 'cypress/videos/**/*.mp4', allowEmptyArchive: true
                }
                success {
                    echo 'Tests passed successfully!'
                }
                failure {
                    echo 'Some tests failed. Check the artifacts for details.'
                }
            }
        }
    }

    post {
        cleanup {
            // Clean up any resources or Docker containers if required
            echo 'Cleaning up...'
            sh 'docker system prune -f'
        }
    }
}

