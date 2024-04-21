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
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    script {
                        // Login to Docker Hub
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                        
                        // Push Docker images to Docker Hub
                        docker.image('mohijeet/client-image:latest').push()
                        docker.image('mohijeet/server-image:latest').push()
                        docker.image('mohijeet/worker-image:latest').push()
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
