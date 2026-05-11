pipeline {
    agent any
    stages {
        stage('Build') { steps { sh 'docker build -t node-app .' } }
        stage('Deploy') { steps { 
            sh 'docker rm -f node-app-container || true'
            sh 'docker run -d --name node-app-container -p 3000:3000 node-app' 
        } }
    }
}
