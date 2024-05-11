pipeline {
    environment {
        // Define the Docker registry, credential ID, and variable to hold the Docker image
        registry = "anusha172000/mlops_assignment_anusha_husnain"
        registryCredential = 'docid' // Corrected to your specified credential ID
        dockerImage = ''
    }
    agent any

    stages {
        stage('Get Dockerfile from GitHub') {
            steps {
                // Check out the main branch from your GitHub repository
                git branch: 'main', url: 'https://github.com/revolutionaryanusha/CICD.git'
            }
        }
        stage('Build Docker image') {
            steps {
                script {
                    // Build the Docker image with a tag based on the Jenkins build number
                    dockerImage = docker.build("${registry}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker image to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub and push the image using the specified credentials
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Send Email Notification') {
            when {
                // Only send an email if the build is on the 'main' branch
                branch 'main'
            }
            steps {
                script {
                    // Send an email notification upon successful build and push
                    emailext (
                        to: 'i202454@nu.edu.pk',
                        subject: "Successful Build and Push",
                        body: "The build and push to Docker Hub were successful.",
                        attachLog: true,
                        mimeType: 'text/plain'
                    )
                }
            }
        }
    }
}
