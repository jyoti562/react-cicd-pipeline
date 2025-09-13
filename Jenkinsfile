pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "jyothi150/react-cicd-pipeline"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/jyoti562/react-cicd-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                // Use withCredentials for secure handling of Docker Hub username/password
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', 
                                                 usernameVariable: 'DOCKER_USER', 
                                                 passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                        docker push $DOCKER_IMAGE:latest
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                    minikube kubectl -- apply -f deployment/deployment.yaml
                    minikube kubectl -- apply -f deployment/service.yaml
                """
            }
        }
    }
}
