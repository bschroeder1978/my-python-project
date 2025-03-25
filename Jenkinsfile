pipeline {
    agent any
    stages {
        stage('Flake') {
            python3 --version
            python3 -m flake8 --version
            # Lancer l'analyse
            python3 -m flake8 . --count --show-source --statistics || true
        }
    }

}