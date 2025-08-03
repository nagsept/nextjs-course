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
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: "${BRANCH}", url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh """
                        docker stop ${APP_NAME} || true
                        docker rm ${APP_NAME} || true
                        docker run -d --name ${APP_NAME} -p 3000:3000 ${DOCKER_IMAGE}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful: App running in container '${APP_NAME}' on port 3000"
        }
        failure {
            echo "❌ Build or deployment failed. Check the logs."
        }
    }
}
