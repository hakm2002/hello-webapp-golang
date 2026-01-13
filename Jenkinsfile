pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
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
                withSonarQubeEnv('SonarQube') {
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

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t dab8106/hellogo .'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
