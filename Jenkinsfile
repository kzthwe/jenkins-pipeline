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
                    sh 'mvn clean package'  // Maven is used as the build automation tool
                }
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    sh 'mvn test'  // Maven is used for running tests
                }
            }
            post {
                always {
                    emailext(
                        to: "${EMAIL_RECIPIENT}",
                        subject: "Jenkins Pipeline Test Stage: ${currentBuild.fullDisplayName}",
                        body: "The test stage has completed.\n\nBuild URL: ${env.BUILD_URL}",
                        attachmentsPattern: 'logs/*.log'
                    )
                }
            }
        }
        stage('Code Analysis') {
            steps {
                script {
                    echo 'Performing code analysis...'
                    sh 'sonar-scanner'  // SonarQube is used for code analysis
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    sh 'snyk test'  // Snyk is used for security scanning
                }
            }
            post {
                always {
                    emailext(
                        to: "${EMAIL_RECIPIENT}",
                        subject: "Jenkins Pipeline Security Scan: ${currentBuild.fullDisplayName}",
                        body: "The security scan stage has completed.\n\nBuild URL: ${env.BUILD_URL}",
                        attachmentsPattern: 'logs/*.log'
                    )
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging server...'
                    sh 'deploy-to-staging.sh'  // Example deployment script to staging (e.g., AWS EC2)
                }
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    sh 'run-integration-tests.sh'  // Integration tests in a staging environment
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production server...'
                    sh 'deploy-to-production.sh'  // Example deployment script to production (e.g., AWS EC2)
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

