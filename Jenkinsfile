pipeline {
    agent any

    environment {
        BACKEND_DIR = 'backend'
        FRONTEND_DIR = 'frontend'
    }

    stages {
        stage('Preparar Ambiente') {
            steps {
                echo 'Verificando Docker e Docker Compose'
                sh 'docker --version'
                sh 'docker-compose --version'
            }
        }

        stage('Build Backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    echo 'Atualizando imagens (pull)'
                    sh 'docker-compose pull || true'
                    echo 'Parando e removendo containers antigos'
                    sh 'docker-compose down -v || true'
                    echo 'Subindo backend com Docker Compose'
                    sh 'docker-compose up -d --build'
                    echo 'Exibindo últimos logs do backend'
                    sh 'docker-compose logs --tail=50'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    echo 'Atualizando imagens (pull)'
                    sh 'docker-compose pull || true'
                    echo 'Parando e removendo containers antigos'
                    sh 'docker-compose down -v || true'
                    echo 'Subindo frontend com Docker Compose'
                    sh 'docker-compose up -d --build'
                    echo 'Exibindo últimos logs do frontend'
                    sh 'docker-compose logs --tail=50'
                }
            }
        }

        stage('Verificar Containers') {
            steps {
                echo 'Listando containers ativos'
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
