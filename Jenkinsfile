pipeline {
    agent any
    stages {
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ghorbelmahdi/simple-fastapi-app .'
            }
        }
       stage('Snyk Test') {
    steps {
        withCredentials([string(credentialsId: 'Snyk', variable: 'SNYK_TOKEN')]) {
            sh 'snyk auth $SNYK_TOKEN'
            sh 'snyk container test ghorbelmahdi/simple-fastapi-app:latest'
        }
    }
}

        stage('Push Docker Image') {
            steps {
                sh 'docker push ghorbelmahdi/simple-fastapi-app:latest'
            }
        }
    }
  
}
