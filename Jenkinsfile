pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']], 
                          userRemoteConfigs: [[url: 'https://github.com/kzthwe/jenkins-pipeline.git']]])
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
                    sh '/opt/homebrew/Cellar/maven/3.9.9/libexec/bin/mvn test'
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh '/opt/homebrew/Cellar/maven/3.9.9/libexec/bin/mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
        
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            }
        }
        
        stage('Send Notification') {
            steps {
                emailext (
                    to: 'katiekhinezt@gmail.com',
                    subject: "Build ${currentBuild.currentResult}: Job '${env.JOB_NAME}' (${env.BUILD_NUMBER})",
                    body: "The build result is ${currentBuild.currentResult}. Please find the build details at: ${env.BUILD_URL}"
                )
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}

