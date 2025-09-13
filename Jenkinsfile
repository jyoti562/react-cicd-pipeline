pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-credentials')  // Jenkins ID for Docker Hub creds
        DOCKER_IMAGE = "your-dockerhub-username/react-cicd-pipeline"
        KUBECONFIG = credentials('kubeconfig') // Optional: if you store kubeconfig in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/react-cicd-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE:latest ."
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                docker push $DOCKER_IMAGE:latest
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl apply -f deployment/deployment.yaml
                kubectl apply -f deployment/service.yaml
                """
            }
        }
    }
}
