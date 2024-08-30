pipeline {
    agent any
    environment {
        EMAIL_RECIPIENT = 'katiekhinezt@gmail.com'
    }
    stages {
        stage('Send Test Email') {
            steps {
                script {
                    echo 'Sending test email...'
                    emailext(
                        to: "${EMAIL_RECIPIENT}",
                        subject: "Jenkins Pipeline Test: ${currentBuild.fullDisplayName}",
                        body: "This is a test email from Jenkins Pipeline.\n\nBuild URL: ${env.BUILD_URL}"
                    )
                }
            }
        }
    }
}

