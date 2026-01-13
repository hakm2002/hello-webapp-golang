pipeline {
    agent any

    environment {
        // Mengambil token dari Jenkins Credentials
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
                script {
                    // Mengambil path instalasi SonarScanner yang bernama 'sonarqube' di Global Tool Configuration
                    def scannerHome = tool 'sonarqube'
                    
                    // Menggunakan environment server SonarQube yang dikonfigurasi di System Jenkins
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=hello-webapp-golang \
                            -Dsonar.sources=. \
                            -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
    }
}
