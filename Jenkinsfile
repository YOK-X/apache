node {

    agent any
    def app

    stage('Clone repository') {

        checkout scm
    }

    stage('Build image') {

        app = docker.build('yok007/web_server')
    }

    stage('Push image') {

        echo '============== start pushing image =================='
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push('new')
        }
    }

    stage('Deploy') {
        echo ' ============== start docker-compose =================='
        sh 'docker-compose -f docker-compose.yml up -d'
    }

}
