pipeline {
    environment {
        registry = "anusha172000/mlops_assignment_anusha_husnain" 
        registryCredential = 'iddocker' 
        dockerImage = ''
    }
    agent any
    stages {
        stage('Get Dockerfile from GitHub') {
            steps {
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
                    // Explicitly define the Docker Hub URL and use credentials to login and push
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        // Push the Docker image using the build number as the tag
                        dockerImage.push("${registry}:${env.BUILD_NUMBER}")
                    }
                }
            }
        }
        stage('Send Email Notification') {
            when {
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
