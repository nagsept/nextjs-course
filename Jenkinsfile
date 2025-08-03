pipeline {
    agent any

    environment {
        APP_NAME = "nextjs-course"
        DOCKER_IMAGE = "nagsept/nextjs-course:latest"
        GIT_REPO = "https://github.com/nagsept/nextjs-course.git"
        BRANCH = "main"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo "🧹 Cleaning workspace..."
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                echo "📥 Cloning code from GitHub..."
                git branch: "${BRANCH}", url: "${GIT_REPO}"
                echo "✅ Checkout complete."
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🐳 Building Docker image manually..."
                    try {
                        // Use --progress=plain for more verbose output
                        sh 'docker build --progress=plain -t ${DOCKER_IMAGE} .'
                        echo "✅ Docker image built successfully."
                    } catch (err) {
                        echo "❌ Docker build failed!"
                        // Show full Docker logs from build if available
                        sh 'cat /root/.npm/_logs/*.log || echo "No NPM logs found."'
                        error("Stopping pipeline: ${err.getMessage()}")
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "🚀 Running Docker container..."
                    sh """
                        docker stop ${APP_NAME} || true
                        docker rm ${APP_NAME} || true
                        docker run -d --name ${APP_NAME} -p 3000:3000 ${DOCKER_IMAGE}
                    """
                    echo "✅ Docker container running."
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Pipeline failed. Check the stage output above."
        }
        success {
            echo "🎉 App deployed successfully at http://<your-server-ip>:3000"
        }
    }
}
