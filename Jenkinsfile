pipeline {
    agent any

    environment {
        IMAGE_NAME = "nodejs-app"
        TAG = "${IMAGE_NAME}:${BUILD_NUMBER}"
    }

    tools {
        nodejs "Node16"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'docker', url: 'https://github.com/scorpenemods/nodejs-jenkins-sample-app.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Build project') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build Docker image') {
            steps {
                sh "docker build -t ${TAG} ."
            }
        }

        stage('Deploy container') {
            steps {
                sh "docker rm -f nodejs-app || true"
                sh "docker run -d --name nodejs-app -p 3000:3000 ${TAG}"
            }
        }
    }

    post {
        always {
            sh "docker image prune -f"
        }
        success {
            echo "Déploiement terminé avec succès"
        }
    }
}
