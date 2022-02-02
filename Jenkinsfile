properties([disableConcurrentBuilds()])

pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
    }
    stages {
        stage("create docker image") {
            steps {
                echo " ============== start building image =================="
                dir ('docker') {
                	sh 'docker build -t yok007/web_server . '
                }
            }
        }   
        stage("docker push") {
            steps {
                docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                echo " ============== start pushing image =================="                
                sh '''
                docker push yok007/web_server:latest
                '''
                }
            }
        }
        stage('Deploy') {
            steps {
                echo " ============== start docker-compose =================="

        sh 'docker-compose -f docker-compose.yml up -d'
            }
        }
    }
}