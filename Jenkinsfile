pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "node_image"
        CONTAINER_NAME = "node_container"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository containing your Dockerfile and app code, explicitly using the 'main' branch
                git branch: 'main', url: 'https://github.com/Jagan6923/node_docker_jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    bat 'docker build -t %DOCKER_IMAGE% .'
                }
            }
        }

        stage('Stop and Remove Previous Container') {
            steps {
                script {
                    // Stop and remove any running containers with the same name
                    bat "docker stop %CONTAINER_NAME% || true"
                    bat "docker rm %CONTAINER_NAME% || true"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container, map port 4000 of the container to 3000 of the host
                    bat "docker run -d -p 3000:3000 --name %CONTAINER_NAME% %DOCKER_IMAGE%"
                }
            }
        }
    }

    post {
        success {
            echo 'Docker container was successfully built and deployed.'
        }
        failure {
            echo 'There was an error with the build process.'
        }
    }
}