pipeline {
    agent any
    stages {
        stage('Unit Testing') {
            steps {
                echo 'Running Unit Tests...'
                sh '''
                    echo "Checking Python version:"
                    python3 --version
                    echo "Checking pytest version:"
                    pytest --version
                    echo "Running pytest..."
                    pytest --junitxml=unit-test-report.xml
                    echo "Listing generated files:"
                    ls -l
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
