pipeline {
    agent { label 'bankapp' }

    environment {
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
                        // Log into Docker Hub (using the safer --password-stdin)
                        echo "Logging into Docker Hub..."
                        sh """
                            echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin
                        """

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
                    echo "Deployment is complete."
                    // deploy()  // Add your deployment logic here
                }
            }
        }
    }
}
