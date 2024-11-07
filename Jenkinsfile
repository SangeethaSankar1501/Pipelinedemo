pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sangeetha1501/simplewebapp"
        DOCKER_TAG = "latest"
        kubeConfigId = 'my-kubeconfig' 
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
                    // Use the kubeconfig stored as "Secret Text" in Jenkins
                    withCredentials([string(credentialsId: kubeConfigId, variable: 'KUBECONFIG_CONTENT')]) {
                        // Write the kubeconfig content (string) to a temporary file
                        writeFile file: '/tmp/kubeconfig', text: KUBECONFIG_CONTENT
                        
                        // Use the kubeconfig for deploying with kubectl
                        sh "kubectl --kubeconfig=/tmp/kubeconfig apply -f deployment.yaml"
                    }
                }
            }
        }
    }
}
