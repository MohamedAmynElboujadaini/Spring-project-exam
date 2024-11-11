pipeline {
    agent any

    stages {
        stage("Build Artefact") {
            steps {
                script {
                    // Clean and build the Maven project
                    sh "mvn clean install"
                }
            }
        }

        stage("Build Docker Images") {
            steps {
                script {
                    // Build Docker images using Docker Compose
                    sh "docker compose build"
                }
            }
        }


        stage("Deploy") {
            steps {
                script {
                    // Deploy services in detached mode
                    sh "docker compose up "
                }
            }
        }
    }

post {
    always {
        // Ensure Docker containers are gracefully stopped and then removed
        echo "Stopping Docker containers..."
        script {
            try {
                // Gracefully stop the containers
                sh "docker compose stop || true"
                
                // Bring down the containers and remove volumes
                sh "docker compose down -v || true"
            } catch (Exception e) {
                echo "Error occurred during Docker cleanup: ${e.getMessage()}"
            }
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
