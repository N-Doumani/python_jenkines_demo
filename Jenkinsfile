pipeline {
    agent any

    environment {
        IMAGE_NAME = "python-demo-app"
        CONTAINER_NAME = "python-demo-container"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo '1. Building Docker image...'
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
            }
        }

        stage('Run Tests') {
            steps {
                echo '2. Running pytest inside container...'
                sh 'docker run --rm ${IMAGE_NAME}:${BUILD_NUMBER} pytest'
            }
        }

        stage('Deploy Locally') {
            steps {
                echo '3. Stopping old container and starting new one...'
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo "Deployed successfully! Check port 5000 on the server."
        }
        failure {
            echo "Pipeline failed! Check Console Output."
        }
    }
}