pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs() // Corrected to cleanWs() for the proper function name
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

        stage('Test Application') { // Moved inside stages
            steps {
                sh 'mvn test'
            }
        }
    }
}
