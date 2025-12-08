pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build . -t react'
            }
        }

        stage('Stop & Clean') {
            steps {
                echo 'Stopping and removing old container...'
                sh 'docker stop react || true'
                sh 'docker rm react || true'
            }
        }
        
        // --- MANUAL APPROVAL STAGE ---
        stage('Manual Approval for Deployment') {
            steps {
                script {
                    // Pauses the pipeline until a user clicks 'Proceed' or 'Abort'
                    // You can specify who is allowed to approve using the 'submitter' parameter.
                    // The 'message' is what the user sees on the screen.
                    input(
                        message: 'Ready to deploy to production? Please approve or abort.',
                        ok: 'Deploy Now', // Custom label for the approval button
                        id: 'DeployConfirmation',
                        // submitter: 'admin,ops-team', // Optional: Restrict who can approve
                        // parameters: [
                        //     string(name: 'REASON', defaultValue: '', description: 'Reason for deploying')
                        // ]
                    )
                }
            }
        }
        // ------------------------------

        stage('Deploy Container') {
            steps {
                echo 'Starting new container on port 80...'
                sh 'docker run -d --name react -p 80:80 react'
            }
        }
    }

    post {
        always {
            echo 'Pipeline job finished.'
        }
    }
}
