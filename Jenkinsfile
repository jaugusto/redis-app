pipeline {
    agent any
    stages {
        stage('build da image docker'){
            steps{
                sh 'docker build -t augustolustosa/redis-app .'
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
        stage('validacao com o sonarqube'){
            steps{
                script{
                    scannerHome = tool 'sonar-scanner';
                }
                withSonarQubeEnv('sonar-server'){
                    sh "${scannerHome}/bin/sonar-scanner  -Dsonar.projectKey=redis-app  -Dsonar.sources=.  -Dsonar.host.url=${env.SONAR_HOST_URL}  -Dsonar.login=${env.SONAR_AUTH_TOKEN}"
                }
            }
        }
        // stage('qualit gate'){
        //     steps{
        //         waitForQualityGate abortPipeline: true
        //     }
        // }
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
        stage('upload docker image'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'nexus-user', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                    sh 'docker login -u $USERNAME -p $PASSWORD ${NEXUS_URL}'
                    sh 'docker tag augustolustosa/nodeapp:latest ${NEXUS_URL}/augustolustosa/nodeapp'
                    sh 'docker push ${NEXUS_URL}/augustolustosa/nodeapp'
                }
                slackSend(message: 'uploading docker image...', sendAsText: true)
            }
        }
    }
}