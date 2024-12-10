pipeline {
    agent any
    stages {
        stage('Unit Testing') {
            steps {
                echo 'Running Unit Tests...'
                sh '''
                    
                    pytest --junitxml=unit-test-report.xml
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ghorbelmahdi/simple-fastapi-app .'
            }
        }
        stage('Snyk Test') {
            steps {
                sh 'snyk container test ghorbelmahdi/simple-fastapi-app:latest'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push ghorbelmahdi/simple-fastapi-app:latest'
            }
        }
    }
    post {
        always {
            junit 'unit-test-report.xml'
        }
    }
}
