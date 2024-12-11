@Library('shared@main')_

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
                dockerbuild("bankapp-mini", "latest")
                echo "Code build bhi ho gaya."
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

