pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://10.211.55.4:9000'
        SONAR_TOKEN = 'squ_54f6daad603d47d3044860ebcb7b4da46c8d41ac'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clonage du repo GitHub
                git branch: 'main', url: 'https://github.com/Hachani-mohamedsaid/jenkinsmohamedsaidhachani4sim1.git'
            }
        }

        stage('Build & Test') {
            steps {
                // Compile le projet et exécute les tests
                sh 'mvn clean verify'
            }
        }

        stage('Analyse SonarQube') {
            steps {
                // Analyse SonarQube avec token
                sh """
                    mvn sonar:sonar \
                    -Dsonar.host.url=${SONAR_HOST_URL} \
                    -Dsonar.token=${SONAR_TOKEN}
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            echo 'La pipeline a échoué. Vérifie les logs.'
        }
    }
}
