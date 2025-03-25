pipeline {
    agent any
    stages {
        stage('Flake') {
            sh 'python3 --version'
            sh 'python3 -m flake8 --version'
            # Lancer l'analyse
            sh 'python3 -m flake8 . --count --show-source --statistics || true'
        }
    }

}