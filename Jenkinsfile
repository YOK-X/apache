pipeline {
agent any

    stages {
        stage('create docker image') {
            steps {
                echo ' ============== start building image =================='

            /* groovylint-disable-next-line SpaceAfterMethodCallName */
            dir ('docker') {
                sh 'docker build -t yok007/web_server . '
            }
            }
        }

        stage('docker push') {
            steps {
                echo ' ============== start pushing image =================='

                /* groovylint-disable-next-line LineLength */
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_REGISTRY_PWD', usernameVariable: 'DOCKER_REGISTRY_USER')]) {
            sh '''
                docker push yok007/web_server:latest
            '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo ' ============== start docker-compose =================='

            sh 'docker-compose -f docker-compose.yml up -d'
            }
        }
    }
}
