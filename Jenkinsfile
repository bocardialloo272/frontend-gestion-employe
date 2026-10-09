pipeline {
    // agent windows 1
    agent {
        label 'agent-windows'
    }

    environment {
        DOCKERHUB_USER = 'bocardocker'
        IMAGE_NAME     = 'frontend-employe'
        IMAGE_TAG      = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKERHUB_USER%/%IMAGE_NAME%:%IMAGE_TAG% -t %DOCKERHUB_USER%/%IMAGE_NAME%:latest ."
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat """
                        docker push %DOCKERHUB_USER%/%IMAGE_NAME%:%IMAGE_TAG%
                        docker push %DOCKERHUB_USER%/%IMAGE_NAME%:latest
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Image frontend construite et poussée avec succès.'
            // À activer quand le job de déploiement existera :
            // build job: 'deploy-pipeline', wait: false
        }
        failure {
            echo 'Le pipeline frontend a échoué, vérifiez les logs Jenkins.'
        }
    }
}