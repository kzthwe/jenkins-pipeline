pipeline {
    agent any
    
    environment {
        EMAIL_RECIPIENT = 'katiekhinezt@gmail.com'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Verify Directory Structure') {
            steps {
                script {
                    sh 'ls -al'
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    sh '/opt/homebrew/Cellar/maven/3.9.9/libexec/bin/mvn clean package'
                }
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                script {
                    // Assuming JUnit for unit tests and integration tests
                    sh '/opt/homebrew/Cellar/maven/3.9.9/libexec/bin/mvn test'
                }
            }
            post {
                always {
                    emailext(
                        to: EMAIL_RECIPIENT,
                        subject: 'Unit and Integration Tests Results',
                        body: 'The unit and integration tests have been completed.',
                        attachmentsPattern: '**/target/surefire-reports/*.xml'
                    )
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                script {
                    // Assuming SonarQube for code analysis
                    sh '/opt/homebrew/Cellar/maven/3.9.9/libexec/bin/mvn sonar:sonar'
                }
            }
            post {
                always {
                    emailext(
                        to: EMAIL_RECIPIENT,
                        subject: 'Code Analysis Results',
                        body: 'The code analysis has been completed.',
                        attachmentsPattern: '**/target/sonar/report-task.txt'
                    )
                }
            }
        }
        
        stage('Security Scan') {
            steps {
                script {
                    // Assuming OWASP Dependency-Check for security scanning
                    sh '/opt/homebrew/Cellar/maven/3.9.9/libexec/bin/mvn org.owasp:dependency-check-maven:check'
                }
            }
            post {
                always {
                    emailext(
                        to: EMAIL_RECIPIENT,
                        subject: 'Security Scan Results',
                        body: 'The security scan has been completed.',
                        attachmentsPattern: '**/target/dependency-check-report.html'
                    )
                }
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                script {
                    // Assuming AWS CLI for deployment to EC2 instance
                    sh 'aws deploy push --application-name my-app --s3-location s3://my-bucket/my-app.zip'
                }
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                script {
                    // Assuming we have a staging environment URL for integration testing
                    sh 'curl -X GET http://staging-server-url/integration-tests'
                }
            }
        }
        
        stage('Deploy to Production') {
            steps {
                script {
                    // Assuming AWS CLI for deployment to EC2 instance
                    sh 'aws deploy push --application-name my-app --s3-location s3://my-bucket/my-app-prod.zip'
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

