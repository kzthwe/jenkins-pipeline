pipeline {
    agent any
    stages {
        stage('Send Test Email') {
            steps {
                script {
                    echo 'Sending test email...'
                    emailext(
                        to: 'katiekhinezt@gmail.com',
                        subject: 'Test Email from Jenkins',
                        body: 'This is a test email sent from Jenkins.',
                        debug: true // Enable debug information
                    )
                }
            }
        }
    }
}

