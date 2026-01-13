pipeline {
    agent any

    tools {
        sonarScanner 'sonar'
    }

    environment {
        SONAR_TOKEN = credentials('hello-webapp-golang')
    }

    stages {

        stage('Check Go') {
            steps {
                sh 'go version'
            }
        }

        stage('Test') {
            steps {
                sh 'go test ./...'
            }
        }

        stage('Build') {
            steps {
                sh 'go build -o app'
                archiveArtifacts artifacts: 'app', fingerprint: true
            }
        }

        stage('Sonar Analysis') {
            steps {
                sh '''
                  sonar-scanner \
                    -Dsonar.projectKey=hello-webapp-golang \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
    }
}
