pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Example build command, replace with actual build tool commands
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Example test command, replace with actual test commands
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running code analysis...'
                // Example code analysis command, replace with actual tool commands
                sh 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Example security scan command, replace with actual tool commands
                sh 'npm audit'
            }
        }

        stage('Run Shell Script') {
            steps {
                echo 'Running shell script...'
                // Replace with the path to your shell script
                sh './path/to/your-script.sh'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                // Example deployment command, replace with actual deployment commands
                sh 'scp target/*.jar user@staging-server:/path/to/deploy'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Example integration test command, replace with actual test commands
                sh 'curl -s http://staging-server/test_endpoint'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                // Example production deployment command, replace with actual deployment commands
                sh 'scp target/*.jar user@production-server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            echo 'This will always run, regardless of the pipeline result'
        }

        success {
            echo 'Pipeline succeeded!'
            emailext subject: 'Jenkins Pipeline Succeeded',
                     body: 'The Jenkins pipeline has completed successfully.',
                     to: 'katiekhinezt@gmail.com',
                     attachLog: true
        }

        failure {
            echo 'Pipeline failed!'
            emailext subject: 'Jenkins Pipeline Failed',
                     body: 'The Jenkins pipeline has failed. Please check the logs.',
                     to: 'katiekhinezt@gmail.com',
                     attachLog: true
        }
    }
}

