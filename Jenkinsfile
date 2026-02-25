pipeline {
    agent any

    environment {
        BACKEND_DIR = 'backend'
        FRONTEND_DIR = 'frontend'
    }

    stages {

        stage('Checkout Backend') {
            steps {
                echo 'Fazendo checkout do backend...'
                dir("${BACKEND_DIR}") {
                    git branch: 'jenkins2', url: 'https://github.com/EmmanuelFLG/edu-gestao-back.git'
                }
            }
        }

        stage('Checkout Frontend') {
            steps {
                echo 'Fazendo checkout do frontend...'
                dir("${FRONTEND_DIR}") {
                    git branch: 'jenkins2', url: 'https://github.com/EmmanuelFLG/edu-gestao-front.git'
                }
            }
        }

        stage('Preparar Ambiente') {
            steps {
                echo 'Verificando Docker e Docker Compose...'
                sh 'docker --version || true'
                sh 'docker-compose --version || true'
            }
        }

        stage('Build Backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    echo 'Atualizando imagens do backend (pull)...'
                    sh 'docker-compose pull || true'
                    echo 'Parando e removendo containers antigos do backend...'
                    sh 'docker-compose down -v || true'
                    echo 'Subindo backend com Docker Compose...'
                    sh 'docker-compose up -d --build'
                    echo 'Exibindo últimos logs do backend...'
                    sh 'docker-compose logs --tail=50'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    echo 'Atualizando imagens do frontend (pull)...'
                    sh 'docker-compose pull || true'
                    echo 'Parando e removendo containers antigos do frontend...'
                    sh 'docker-compose down -v || true'
                    echo 'Subindo frontend com Docker Compose...'
                    sh 'docker-compose up -d --build'
                    echo 'Exibindo últimos logs do frontend...'
                    sh 'docker-compose logs --tail=50'
                }
            }
        }

        stage('Verificar Containers') {
            steps {
                echo 'Listando containers ativos...'
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo 'Build e deploy concluídos com sucesso!'
        }
        failure {
            echo 'Houve erro no build ou deploy!'
        }
    }
}
