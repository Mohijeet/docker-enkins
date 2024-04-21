pipeline {
    agent any
    
    stages {
        stage('Build Docker images') {
            steps {
                script {
                    // Build Docker images from Dockerfiles in the client, server, and worker directories
                    docker.build('mohijeet/client-image', './client')
                    docker.build('mohijeet/server-image', './server')
                    docker.build('mohijeet/worker-image', './worker')
                    // Build other Docker images if needed
                }
            }
        }
        
        stage('Push Docker images to Docker Hub') {
            steps {
                script {
                    // Push Docker images to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                        docker.image('mohijeet/client-image').push('latest')
                        docker.image('mohijeet/server-image').push('latest')
                        docker.image('mohijeet/worker-image').push('latest')
                        // Push other Docker images if needed
                    }
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Deploy the Docker images using Docker Compose
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
