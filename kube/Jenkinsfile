pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps{
        script {
          checkout scm
        }
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build("vladsanyuk/ssdevopscc:php-httpd")
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {

          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'kubectl apply -f lamp-secret.yaml'
            sh 'kubectl apply -f mariadb-configmap.yaml'
            sh 'kubectl apply -f mariadb.yaml'
            sh 'kubectl apply -f php-httpd.yaml'
            sh 'kubectl apply -f phpmyadmin.yaml'
        }
      }
    }
  }
}