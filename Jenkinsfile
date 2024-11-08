pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sangeetha1501/simplewebapp"
        DOCKER_TAG = "latest"
        kubeConfigId = '/home/sakshara479/.kube/config' 
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub or another source
                git branch: 'main', url: 'https://github.com/SangeethaSankar1501/Pipelinedemo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log into Docker Hub
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }

                    // Push the Docker image to Docker Hub
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                        sh "kubectl apply -f deployment.yaml"
                    }
                }
            }
        }
    }
}
