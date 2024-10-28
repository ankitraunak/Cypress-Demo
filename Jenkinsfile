pipeline {
    agent {
        docker {
            image 'cypress/browsers'  // Use the Cypress image with necessary browsers
            args '-u root'            // Run as root for permissions management
        }
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


}
