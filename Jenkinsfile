pipeline {
    agent any
    environment {
        EMAIL_RECIPIENT = 'katiekhinezt@gmail.com'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the code...'
                    // Example: sh 'mvn clean package'
                }
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    // Example: sh 'mvn test'
                }
            }
        }
        stage('Code Analysis') {
            steps {
                script {
                    echo 'Performing code analysis...'
                    // Example: sh 'sonar-scanner'
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    // Example: sh 'snyk test'
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging server...'
                    // Example: sh 'deploy-to-staging.sh'
                }
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    // Example: sh 'run-integration-tests.sh'
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production server...'
                    // Example: sh 'deploy-to-production.sh'
                }
            }
        }
    }
    post {
        success {
            emailext(
                to: "${EMAIL_RECIPIENT}",
                subject: "Jenkins Pipeline Success: ${currentBuild.fullDisplayName}",
                body: "The pipeline has completed successfully.\n\nBuild URL: ${env.BUILD_URL}",
                attachmentsPattern: 'logs/*.log'
            )
        }
        failure {
            emailext(
                to: "${EMAIL_RECIPIENT}",
                subject: "Jenkins Pipeline Failed: ${currentBuild.fullDisplayName}",
                body: "The pipeline has failed.\n\nBuild URL: ${env.BUILD_URL}",
                attachmentsPattern: 'logs/*.log'
            )
        }
    }
}

