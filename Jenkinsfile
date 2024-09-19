pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs() // Corrected to cleanWs() for workspace cleanup
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/AnithaKuberappa/register-app1.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test Application') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') { // Added a separate stage for clarity
            steps {
                script {
                    withSonarQubeEnv('Jenkins-Sonarqube-token') { // Corrected placement of credentials
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
    }
}
