pipeline {
agent any

    stages {
        stage("docker login") {
            steps {
                echo " ============== docker login =================="
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                    docker login -u $USERNAME -p $PASSWORD
                    """
                }
            }
        }
        stage('create docker image') {
            steps {
                echo ' ============== start building image =================='

            dir ('docker') {
                sh 'docker build -t yok007/web-server . '
            }
            }
        }

        stage('docker push') {
            steps {
                echo ' ============== start pushing image =================='


            sh '''docker push yok007/web-server'''
                }
            }

        stage('Deploy') {
            steps {
                echo ' ============== start docker-compose =================='

            sh 'docker-compose -f docker/docker-compose.yml up -d'
            }
        }
    }
}
