pipeline {
    agent any
    triggers {
        githubPush()
    }
    stages {
        stage('Build Docker Image'){
            steps{
                sh 'docker build . -t react'
            }
        }
        stage('Stop & clean'){
            steps{
                echo 'Stopping & removing old conatainer....'
                sh 'docker stop react || true'
                sh 'docker rm react || true'
            }
        }
        stage('Deploy Container'){
            steps{
                echo 'Starting new container on port 80...'
                sh 'docker run -d --name react -p 8081:80 react'
            }
        }
    }
    post {
        always {
            echo 'Pipeline job finished.'
        }
    }
}
