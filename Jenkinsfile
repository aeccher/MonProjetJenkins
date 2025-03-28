pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "aeccher/monprojetjenkins"
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git 'https://github.com/aeccher/MonProjetJenkins.git'
            }
        }

        stage('Installer les dépendances') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Exécuter les tests unitaires') {
            steps {
                sh 'pytest'
            }
        }

        stage('Construire l’image Docker') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Pousser l’image Docker') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Déployer sur le serveur de test') {
            steps {
                sh '''
                ssh user@server "docker pull $DOCKER_IMAGE && docker run -d -p 8000:8000 $DOCKER_IMAGE"
                '''
            }
        }
    }
}
