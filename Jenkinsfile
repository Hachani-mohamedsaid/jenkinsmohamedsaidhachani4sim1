pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Hachani-mohamedsaid/jenkinsmohamedsaidhachani4sim1.git'
            }
        }

        stage('Build') {
            steps {
                echo "Build terminé"
            }
        }

        stage('Test') {
            steps {
                echo "Tests exécutés"
            }
        }
    }
}
