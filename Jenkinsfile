pipeline {
    agent any
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
        // Other stages like Tests, Deployments, etc.
    }
    post {
        always {
            echo 'Pipeline complete.'
        }
    }
}

