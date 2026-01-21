pipeline { 
    agent any

environment {
        IMAGE_NAME = "sumitdorugade/cicd"
        DOCKERHUB_REPO = "sumitdorugade"
    }

stages {
    stage('Checkout Code') {
        steps {
            echo '📦 Cloning repository...'
            git branch: 'main', url: 'https://github.com/Sumit19D/CI-CD-Pipeline-using-Jenkins-Docker-and-Kubernetes.git'
        }
    }

    stage('Build Docker Images') {
            steps {
                script {
                    echo '🐳 Building Docker images...'
                    sh """
                        docker build -t ${DOCKERHUB_REPO}/devops-backend:latest ./Backend
                        docker build -t ${DOCKERHUB_REPO}/devops-frontend:latest ./Frontend
                    """
                }
            }
        }

    stage('Push Images to DockerHub') {
        steps {
            script {
                echo '📤 Pushing Docker images to DockerHub...'
                withCredentials([usernamePassword(credentialsId: "3df4239f-7ded-44d7-96dd-e6b7765ed941", usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                        echo "$PASS" | docker login -u "$USER" --password-stdin

                        docker push ${DOCKERHUB_REPO}/devops-backend:latest
                        docker push ${DOCKERHUB_REPO}/devops-frontend:latest

                        docker logout
                    """
                }
            }
        }
    }

    stage('Deploy to Kubernetes') {
        steps {
           script {
            echo '🚀 Deploying application to Kubernetes...'
            withCredentials([file(credentialsId: '6918a57b-f784-4d9b-be1c-3e1ed67f17bb', variable: 'KUBECONFIG')]) {
            sh """
                
                # Apply manifests (skip validation to avoid cert issues)
                kubectl apply -f K8S/backend-deployment.yaml --validate=false
                kubectl apply -f K8S/frontend-deployment.yaml --validate=false
                kubectl apply -f K8S/service.yaml --validate=false

                # Update deployments with new images
                kubectl set image deployment/devops-backend backend=${DOCKERHUB_REPO}/devops-backend:latest --record
                kubectl set image deployment/devops-frontend frontend=${DOCKERHUB_REPO}/devops-frontend:latest --record

                # Wait for rollout
                kubectl rollout status deployment/devops-backend
                kubectl rollout status deployment/devops-frontend
            """
        }
    }
}
}
    post {
    success {
        echo "✅ Pipeline completed successfully! Images pushed and deployed with tag $IMAGE_NAME:latest"
    }
    failure {
        echo "❌ Pipeline failed. Check logs for details."
    }
}
}
}
