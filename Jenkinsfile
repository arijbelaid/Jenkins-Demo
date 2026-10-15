pipeline {
    agent any

    tools {
        jdk 'jdk-21'
        maven 'maven-3.9.12'
    }

    environment {
        DOCKER_IMAGE = "arijbelaid/jenkins-demo:${env.BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repo') {
            steps {
                echo '🔄 Récupération du dépôt GitHub...'
                checkout scm
                sh 'git log -1 --oneline'
            }
        }

        stage('Build Project') {
            steps {
                echo '📦 Build Maven...'
                sh 'mvn clean package -DskipTests'
                sh 'ls -la target/'
            }
        }

        stage('Fix Dockerfile') {
            steps {
                echo '🔧 Correction du Dockerfile (openjdk → eclipse-temurin)...'
                sh '''
                    sed -i 's|openjdk:17-jdk-slim|eclipse-temurin:17-jre|g' Dockerfile
                    sed -i 's|openjdk:17-jdk|eclipse-temurin:17-jre|g' Dockerfile
                    echo "=== Contenu du Dockerfile corrigé ==="
                    cat Dockerfile
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Construction de l'image Docker ${DOCKER_IMAGE}..."
                sh "docker build -t ${DOCKER_IMAGE} ."
                sh 'docker images | grep jenkins-demo'
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo '🚀 Déploiement du conteneur Docker...'
                sh '''
                    docker stop devopsexample || true
                    docker rm devopsexample || true
                    docker run --name devopsexample -d -p 2222:2222 arijbelaid/jenkins-demo:${BUILD_NUMBER}
                '''
            }
        }

        stage('Verify') {
            steps {
                echo '✅ Vérification de l\'application...'
                sh '''
                    sleep 10
                    docker ps | grep devopsexample
                    curl -s http://localhost:2222 | head -20 || echo "Application en cours de démarrage"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline terminé avec succès !'
            echo "Image Docker : arijbelaid/jenkins-demo:${env.BUILD_NUMBER}"
        }
        failure {
            echo '❌ Le pipeline a échoué.'
        }
        always {
            echo "Build #${env.BUILD_NUMBER} terminé"
        }
    }
}
