pipeline {
    agent any

    environment {
        IMAGE_NAME = 'joalias/python-app:latest'
    }

    stages {
        stage('clone') {
            steps {
                echo 'Clonando el proyecto...'
                checkout scm
            }
        }

        stage('test') {
            steps {
                echo 'Instalando Flask y Pytest y ejecutando pruebas...'
                sh '''
                    python3 -m pip install --break-system-packages flask pytest
                    pytest test_app.py
                '''
            }
        }

        stage('build image') {
            steps {
                echo 'Construyendo la imagen Docker...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('dockerhub') {
            steps {
                echo 'Subiendo la imagen a DockerHub...'
                sh '''
                    docker login -u joalias -p dckr_pat_oJILs4-oViJUyErnPSBGz7GIUCM
                    docker push $IMAGE_NAME
                '''
            }
        }

        stage('deploy') {
            steps {
                echo 'Desplegando la aplicación en Kubernetes...'
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                '''
            }
        }
    }
}
// Comentario para lanzar de nuevo el pipeline
