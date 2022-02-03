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
                    /* groovylint-disable-next-line NoDef, VariableTypeRequired */
                    def registry_url = 'registry.hub.docker.com/'
                    bat "docker login -u $USER -p $PASSWORD ${registry_url}"
                    docker.withRegistry("http://${registry_url}", "docker-hub-credentials")
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
