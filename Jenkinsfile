pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/madieume/examenDevOps.git'
        BRANCH = 'main'
        RENDER_API_KEY = credentials('render-api-key') // clé API stockée dans Jenkins
        RENDER_SERVICE_ID = 'ton-service-id-render'
    }

    stages {
        stage('Cloner le projet') {
            steps {
                git branch: "${BRANCH}", url: "${GIT_REPO}"
            }
        }

        stage('Build & Tests') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('Archiver Artefact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Déploiement sur Render') {
            steps {
                bat '''
                curl -X POST "https://api.render.com/v1/services/${RENDER_SERVICE_ID}/deploys" \
                -H "Authorization: Bearer ${RENDER_API_KEY}" \
                -H "Content-Type: application/json"
                '''
            }
        }
    }

    post {
        success {
            echo ' Pipeline exécuté avec succès'
        }
        failure {
            echo ' Le pipeline a échoué'
        }
    }
}
