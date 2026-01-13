pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar_token')
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
                sh 'go build -o main'
                archiveArtifacts 'main'
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
