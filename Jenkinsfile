pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs() // Cleanup the workspace before starting
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/AnithaKuberappa/register-app1.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package' // Build the application using Maven
            }
        }

        stage('Test Application') {
            steps {
                sh 'mvn test' // Run tests using Maven
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId:'Sonarqube-server') { // Use the SonarQube server name, not credentialsId
                        sh 'mvn sonar:sonar' // Run SonarQube analysis
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId:'Sonarqube-server'
                }
            }
        }
    }
}
