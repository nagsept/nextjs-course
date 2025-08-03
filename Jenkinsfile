pipeline {
    agent any

    environment {
        APP_NAME = "nextjs-course"
        DOCKER_IMAGE = "nagsept/nextjs-course:latest"
        GIT_REPO = "https://github.com/nagsept/nextjs-course.git"
        BRANCH = "main"
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Clean workspace before fresh clone
                    deleteDir()
                }
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

        stage('Deploy Container') {
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
        failure {
            echo "Build or deployment failed. Please check the logs."
        }
        success {
            echo "Next.js app deployed successfully in Docker container: ${APP_NAME}"
        }
    }
}
