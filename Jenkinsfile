pipeline {
    agent any
    stages {
        stage('build da image docker'){
            steps{
                sh 'docker build -t augustolustosa/node-app .'
                slackSend(message: "build image - OK", sendAsText: true)
            }
        }
        stage('subir docker compose - redis e app'){
            steps{
                sh 'docker-compose up --build -d'
                slackSend(message: "upload image - OK", sendAsText: true)
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
                slackSend(message: "run test app - OK", sendAsText: true)
            }
        }
        stage('testando integracao com slack'){
            steps{
                slackSend(message: "Finished - OK", sendAsText: true)
            }
        }
        stage('Shutting down containers'){
            steps{
                sh 'docker-compose down'
                slackSend(message: 'shutting down containers', sendAsText: true)
            }
        }
    }
}