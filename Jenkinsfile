pipeline {
    agent any
    stages {
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ghorbelmahdi/simple-fastapi-app .'
            }
        }
       stage('Trivy Scan') {
            steps {
                sh 'trivy image ghorbelmahdi/simple-fastapi-app:latest'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                   sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                   docker push ghorbelmahdi/simple-fastapi-app:latest
                      '''
}

            }
        }
    }
  
}
