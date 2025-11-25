pipeline {
    agent any

    environment {
        // 👉 Ton vrai username Docker Hub
        DOCKERHUB_USERNAME = 'hachanimohamedsaid'

        // 👉 Nom de l'image Docker à publier
        DOCKERHUB_IMAGE = "${DOCKERHUB_USERNAME}/student-management"
    }

    stages {
        stage('Clone Repo') {
            steps {
                // 👉 Ton vrai repo GitHub
                git branch: 'main', url: 'https://github.com/Hachani-mohamedsaid/jenkinsmohamedsaidhachani4sim1.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKERHUB_IMAGE}:latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',  // 👉 CORRIGÉ : utiliser l'ID défini dans Jenkins
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh "docker push ${DOCKERHUB_IMAGE}:latest"
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
            sh 'docker image prune -f || true'
        }
    }
}
