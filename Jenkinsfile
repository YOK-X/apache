pipeline {
agent any

    stages {

        stage('create docker image') {
            steps {
                echo ' ============== start building image =================='
                dir ('docker') {
                    sh 'docker build -t yok007/web-server:latest . '
                }
            }
        }

        stage('Scan with trivy') {
            steps {
                sh 'trivy --no-progress --exit-code 1 --severity HIGH,CRITICAL darinpope/java-web-app:latest'
            }
        }

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
