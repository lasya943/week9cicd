pipeline {
    agent any

    environment {
        // Point kubectl to kubeconfig inside Jenkins workspace
        KUBECONFIG = "${WORKSPACE}\\.kube\\config"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image..."
                bat "docker build -t kubdemoapp:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                echo "Logging in to Docker Hub..."
                // You can replace these with Jenkins credentials for security
                bat 'docker login -u lnalagandla -p tadkamadla1.'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Tagging and pushing Docker Image..."
                bat "docker tag kubdemoapp:v1 lnalagandla/samplew9:kubeimage1"
                bat "docker push lnalagandla/samplew9:kubeimage1"
            }
        }

        stage('Deploy to Kubernetes') { 
            steps { 
                echo "Deploying to Kubernetes..."
                // Apply deployment and service using workspace kubeconfig
                bat 'set KUBECONFIG=%WORKSPACE%\\.kube\\config && kubectl apply -f deployment.yaml --validate=false'
                bat 'set KUBECONFIG=%WORKSPACE%\\.kube\\config && kubectl apply -f service.yaml'
                // Wait until deployment is ready
                bat 'set KUBECONFIG=%WORKSPACE%\\.kube\\config && kubectl rollout status deployment/kubdemoapp'
            } 
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
