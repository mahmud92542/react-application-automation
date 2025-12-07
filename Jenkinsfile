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
