pipeline { 
    agent any

environment {
        IMAGE_NAME = "sumitdorugade/CICD"
        DOCKERHUB_REPO = "sumitdorugade"
    }

stages {
    stage('Checkout Code') {
        steps {
            echo '📦 Cloning repository...'
            git branch: 'main', url: 'https://github.com/Sumit19D/CI-CD-Pipeline-using-Jenkins-Docker-and-Kubernetes.git'
        }
    }

    stage('Build Backend Image') {
        steps {
            script {
                echo '🐳 Building backend Docker image...'
                sh """
                    docker build -t $IMAGE_NAME:latest  ./backend
                    docker tag $IMAGE_NAME:latest  ${DOCKERHUB_REPO}/devops-backend:latest
                """
            }
        }
    }

    stage('Build Frontend Image') {
        steps {
            script {
                echo '🐳 Building frontend Docker image...'
                sh """
                    docker build -t $IMAGE_NAME:latest ./frontend
                    docker tag $IMAGE_NAME:latest ${DOCKERHUB_REPO}/devops-frontend:latest
                """
            }
        }
    }

    stage('Push Images to DockerHub') {
        steps {
            script {
                echo '📤 Pushing Docker images to DockerHub...'
                withCredentials([usernamePassword(credentialsId: "dockerhub-creds", usernameVariable: 'USER', passwordVariable: 'PASS')]) {
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
            echo '🚀 Deploying application to Kubernetes...'
            sh """
                export KUBECONFIG="/var/lib/jenkins/.kube/config"

                # Apply manifests (skip validation to avoid cert issues)
                kubectl apply -f k8s/backend-deployment.yaml --validate=false
                kubectl apply -f k8s/frontend-deployment.yaml --validate=false
                kubectl apply -f k8s/service.yaml --validate=false

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

post {
    success {
        echo "✅ Pipeline completed successfully! Images pushed and deployed with tag ${IMAGE_TAG}"
    }
    failure {
        echo "❌ Pipeline failed. Check logs for details."
    }
}
}
