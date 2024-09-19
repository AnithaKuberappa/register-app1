pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
	    APP_NAME = "register-app1-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "anithakuberappa"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    #JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
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
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

          }
    }
}
