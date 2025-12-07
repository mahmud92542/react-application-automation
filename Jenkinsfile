// Jenkinsfile
pipeline {
    
    // Agent must be configured to have Docker access (jenkins user in docker group)
    agent any 

    // Define triggers if not using the GUI (good for code-based configuration)
    triggers {
        // This is the trigger that responds to the GitHub webhook
        githubPush() 
    }

    stages {
        
        stage('Checkout Code') {
            steps {
                // Ensure the latest code is pulled
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the image and tag it 'react'
                sh 'docker build . -t react'
            }
        }

        stage('Stop & Clean') {
            steps {
                // Stop the running container. '|| true' prevents pipeline failure if the container is not found.
                echo 'Stopping and removing old container...'
                sh 'docker stop react || true'
                // Remove the container instance. '|| true' prevents pipeline failure.
                sh 'docker rm react || true'
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Starting new container on port 3000...'
                // Run the new container in detached mode (-d) with the port mapping
                sh 'docker run -d --name react -p 3000:3000 react'
            }
        }
    }

    post {
        // Runs regardless of success or failure
        always {
            echo 'Pipeline job finished.'
        }
    }
}
