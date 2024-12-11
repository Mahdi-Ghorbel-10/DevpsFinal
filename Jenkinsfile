pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Mahdi-Ghorbel-10/DevpsFinal.git', branch: 'main'
            }
        }
        stage('Unit Testing') {
            steps {
                sh '''
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install -r requirements.txt
                    pytest --junitxml=unit-test-report.xml
                '''
            }
        }
        
        
        
       
       
      stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'; // Ensure 'SonarScanner' matches your configured scanner name in Jenkins
                    withCredentials([string(credentialsId: 'soanr_auth', variable: 'SONAR_AUTH_TOKEN')]) {
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=projetDeVOPS \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=$SONAR_AUTH_TOKEN"
                    }
                }
            }
        }
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
