pipeline {
    agent any
    environment {
        DOCKER_LOGIN=bschroeder1978
    }
    stages {
        stage('Flake') {
            steps {
                script {
                    sh 'python3 --version'
                    sh 'python3 -m flake8 --version'
                    // Lancer l'analyse
                    sh 'python3 -m flake8 . --count --show-source --statistics || true'
                }
            }
        }
        stage('Test') {
            steps {
                //pip install -r requirements.txt
                //pip install pytest
                sh 'pytest | tee report.txt'
            }
        }
        stage('Docker Publish') {
            steps {
                withCredentials([string(credentialsId: 'BSC_DOCKER_PASSWORD', variable: 'DOCKER_PASS')]) {
                    //Construire l'image Docker avec le BUILD_NUMBER de Jenkins 
                    docker build -t bschroeder1978/my-python-project:$BUILD_NUMBER . 
                    // Se connecter à Docker Hub en utilisant le token d'accès 
                    docker login -u $DOCKER_LOGIN -p $DOCKER_PASS 
                    //Pousser l'image sur Docker Hub 
                    docker push bschroeder1978/my-python-project:$BUILD_NUMBER 
                }
            }
        }
    }

}