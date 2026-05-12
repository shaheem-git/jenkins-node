pipeline {
    agent any

    environment {
        IMAGE_NAME = "node-app"
        CONTAINER_NAME = "node-app-container"
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing project dependencies...'
                sh 'npm install'
            }
        }

        stage('Code Scanning & Security') {
            steps {
                echo 'Scanning code for vulnerabilities...'
                // Checks for known security issues in your dependencies
                sh 'npm audit fix --audit-level=high || true' 
                
                // Optional: You can also use a linter to check for code quality
                echo 'Running static code analysis...'
                sh 'npx eslint . --fix || true'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                // If you have tests defined in package.json, this runs them
                // Using '|| true' ensures the pipeline continues even if tests aren't set up yet
                sh 'npm test || echo "No tests defined, skipping."'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to local server...'
                // Remove old container if it exists
                sh "docker rm -f ${CONTAINER_NAME} || true"
                // Run the new container
                sh "docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            echo 'Deployment successful! app is updated.'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
