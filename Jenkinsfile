pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred') // ID de tes credentials DockerHub
        IMAGE_NAME = "madieume/devopexam"
        APP_PORT = 8083
        RENDER_SERVICE_ID = "srv-d37a0qggjchc73c3nq90"
        RENDER_API_KEY = credentials('render-api-key') // ID de credentials Render
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'madieume', url: 'https://github.com/madieume/examenDevOps'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy to Render') {
            steps {
                script {
                    sh """
                    curl -X POST "https://api.render.com/deploy/srv-${RENDER_SERVICE_ID}/manual" \\
                    -H "Authorization: Bearer ${RENDER_API_KEY}"
                    """
                }
            }
        }
    }
}
