pipeline {
    agent any

    stages {
        stage("Build Artefact") {
            steps {
                echo "Starting the build process..."
                bat "mvn clean install"
            }
        }

        stage("Build Docker Images") {
            steps {
                script {
                    // Build Docker images using Docker Compose
                    bat "docker-compose build"
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    // Deploy services in detached mode using Docker Compose
                    bat "docker-compose up -d"
                }
            }
        }
    }

    post {
        always {
            echo "Stopping Docker containers..."
            script {
                // Gracefully stop the containers and remove volumes using Docker Compose
                bat "docker-compose stop || true"
                bat "docker-compose down -v || true"
            }
        }
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline execution failed!"
        }
    }
}
