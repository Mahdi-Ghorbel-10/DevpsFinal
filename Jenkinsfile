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
       
      stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'; // Ensure 'SonarScanner' matches the name configured in Jenkins
                    withSonarQubeEnv('sq1') { // 'sq1' is the configured SonarQube server name
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=projetDeVOPS \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://your-sonarqube-server:9000 \
                            -Dsonar.login=squ_b99cbc07a2adc9d5ab6532f690f65a40428463e4"
                    }
                }
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
