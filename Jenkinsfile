pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo '🔵 Cloning repository...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo '🔵 Building Docker image...'
                    bat 'docker build -t frontend-app:latest .'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    echo '🔵 Running Docker container...'
                    bat 'docker run -d -p 5000:5000 frontend-app:latest'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        echo '🔵 Logging into Docker Hub and pushing image...'
                        bat '''
                            echo %DOCKER_PASS% | docker login --username %DOCKER_USER% --password-stdin
                            docker tag frontend-app:latest %DOCKER_USER%/frontend-app:latest
                            docker push %DOCKER_USER%/frontend-app:latest
                        '''
                    }
                }
            }
        }

        stage('Done') {
            steps {
                echo '✅ Build, Run, and Push Complete!'
            }
        }
    }
}
