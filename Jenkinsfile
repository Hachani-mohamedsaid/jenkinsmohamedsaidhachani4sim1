pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Analyse SonarQube') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.host.url=http://10.211.55.4:9000 -Dsonar.token=squ_54f6daad603d47d3044860ebcb7b4da46c8d41ac'
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé'
        }
        failure {
            echo 'La pipeline a échoué. Vérifie les logs.'
        }
        success {
            echo 'La pipeline a réussi!'
        }
    }
}
