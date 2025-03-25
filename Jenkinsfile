pipeline {
    agent any
    environment {
        DOCKER_LOGIN='bschroeder1978'
    }
    stages {
        stage('Flake&Test') {
            parallel {  
                stage('Flake') {
                    steps {
                        script {
                            sh 'python3 --version'
                            sh 'python3 -m flake8 --version'
                            // Lancer l'analyse
                            sh '(python3 -m flake8 . --count --show-source --statistics || true) | tee flake.txt'
                            archiveArtifacts artifacts: 'flake.txt', fingerprint: true
                        }
                    }
                }
                stage('Test') {
                    steps {
                        sh 'pytest | tee report.txt'
                        archiveArtifacts artifacts: 'report.txt', fingerprint: true
                    }
                }
            }
        }
        stage('Docker Publish') {
            steps {
                withCredentials([string(credentialsId: 'BSC_DOCKER_PASSWORD', variable: 'DOCKER_PASS')]) {
                    //Construire l'image Docker avec le BUILD_NUMBER de Jenkins 
                    sh 'docker build -t bschroeder1978/my-python-project:$BUILD_NUMBER .'
                    // Se connecter à Docker Hub en utilisant le token d'accès 
                    sh 'docker login -u $DOCKER_LOGIN -p $DOCKER_PASS' 
                    //Pousser l'image sur Docker Hub 
                    sh 'docker push bschroeder1978/my-python-project:$BUILD_NUMBER'
                }
            }
        }
    }
}