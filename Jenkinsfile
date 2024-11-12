pipeline {
    agent any

    stages {
        stage("Build Artefact") {
            steps {
                echo "Starting the build process..."
                bat "mvn clean install"
            }
        }
                stage("Test Vulnerabilities With SonarQube") {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                    bat """
mvn clean verify sonar:sonar \
  -Dsonar.projectKey=sonarspring \
  -Dsonar.projectName='sonarspring' \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.token=sqp_630b0d66ea4c8f287025e823080e63701c0d0031
                    """
                }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
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
