pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/hpt12/demo-counter-app.git'
            }
        }
        stage('UNIT testing') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Integration testing') {
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('Maven build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv(credentialsId: 'server-api-key', url: 'http://44.204.91.86:9000/') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
        stage('Quality Gate Status') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'server-api-key'
                }
            }
        }
    }
}
