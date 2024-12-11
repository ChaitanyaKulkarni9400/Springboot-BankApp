pipeline {
    agent { label 'bankapp' }

    environment {
        // Define the Docker image name and tag
        DOCKER_IMAGE_NAME = 'bankapp-mini'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Clone the repository
                checkout scm
                echo "Code cloning done."
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Ensure Docker is available on your Jenkins agent
                    sh 'docker --version'

                    // Build Docker image
                    echo "Building Docker image..."
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    // Use credentials securely for Docker login
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
                                                      usernameVariable: 'DOCKER_USERNAME', 
                                                      passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Log into Docker Hub
                        echo "Logging into Docker Hub..."
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"

                        // Push the Docker image to Docker Hub
                        echo "Pushing Docker image to Docker Hub..."
                        sh "docker push ${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                    }
                }
            }
        }

        stage('Deploying') {
            steps {
                script {
                    // Placeholder for your deploy step
                    // Assuming you have a deploy function or script
                    echo "Deployment is complete."
                    deploy()  // Replace this with your actual deployment command
                }
            }
        }
    }
}

