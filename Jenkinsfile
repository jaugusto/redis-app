pipeline {
    agent any
    stages {
        stage('build da image docker'){
            steps{
                sh 'docker build -t augustolustosa/node-app .'
            }
        }
        stage('subir docker compose - redis e app'){
            steps{
                sh 'docker-compose up --build -d'
            }
        }
        stage('sleep para subida dos containers'){
            steps{
                sh 'sleep 10'
            }
        }
        stage('teste da aplicacao'){
            steps{
                sh 'chmod +x teste-app.sh'
                sh './teste-app.sh'
            }
        }
    }
}