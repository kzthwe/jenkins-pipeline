pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'katiekhinezt@gmail.com'
        MAVEN_HOME = tool name: 'Maven 3.9.9', type: 'maven'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                script {
                    echo 'Checking out source code...'
                    checkout scm
                }
            }
        }

        stage('Verify Directory Structure') {
            steps {
                script {
                    echo 'Listing contents of workspace to verify directory structure...'
                    sh 'ls -al'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the code...'
                    // Ensure Maven is executed from the correct directory
                    sh "${MAVEN_HOME}/bin/mvn clean package"
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    // Run unit tests and integration tests using Maven
                    sh "${MAVEN_HOME}/bin/mvn test"
                    sh "${MAVEN_HOME}/bin/mvn verify" // Run integration tests if needed
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    echo 'Performing code analysis...'
                    // Perform code analysis using SonarQube
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    // Perform security scan using Snyk
                    sh 'snyk test'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging server...'
                    // Deploy to staging server (e.g., AWS EC2 instance)
                    sh 'aws deploy push --application-name my-app --s3-location s3://my-bucket/my-app.zip'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    // Run integration tests on staging environment
                    sh 'selenium-server -role hub'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production server...'
                    // Deploy to production server (e.g., AWS EC2 instance)
                    sh 'aws deploy push --application-name my-app-prod --s3-location s3://my-bucket-prod/my-app.zip'
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

