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
            // Ensure Docker containers are stopped after the pipeline execution
            echo "Cleaning up Docker containers..."
            sh "docker compose down || true"
        }
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline execution failed!"
        }
    }
}
