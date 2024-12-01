@Library('shared')_

pipeline {
    agent { label 'bankapp' }

    stages {
        stage("Code") {
            steps {
                // Clone the repository
                script {
                    clone("https://github.com/ChaitanyaKulkarni9400/Springboot-BankApp.git", "main")
                }
                echo "Code cloning done."
            }
        }

        stage("Build") {
            steps {
                // Build the Docker image
                script {
                    dockerbuild("bankapp-mini", "latest")
                }
                echo "Code build is done."
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    // Use DockerHub credentials for login and push
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-creds', 
                        usernameVariable: 'DOCKER_USERNAME', 
                        passwordVariable: 'DOCKER_PASSWORD'
                    )]) {
                        sh """
                            echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                            docker tag bankapp-mini:latest $DOCKER_USERNAME/bankapp-mini:latest
                            docker push $DOCKER_USERNAME/bankapp-mini:latest
                        """
                    }
                }
                echo "Push to DockerHub is complete."
            }
        }

        stage("Deploying") {
            steps {
                // Deploy the application
                script {
                    deploy()
                }
                echo "Deployment is complete."
            }
        }
    }
}

