pipeline {
    agent any

    tools {
        sonarScanner 'sonar'
    }

    environment {
        SONAR_TOKEN = credentials('sonar')
    }

    stages {

        stage('Test') {
            steps {
                sh '''
                  go version
                  go test ./...
                '''
            }
        }

        stage('Build Binary') {
            steps {
                sh '''
                  go build -o main
                '''
                archiveArtifacts artifacts: 'main', fingerprint: true
            }
        }

        stage('Sonar Analysis') {
            steps {
                withSonarQubeE
