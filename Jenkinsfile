pipeline {
    agent any
    tools {
        terraform 'terraform'
    }

    stages {
        stage('Hello') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/terra']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/YOK-X/apache']]])
            }
        }
        
        stage ('Terraform init') {
            steps {
                sh ('terraform init');
            }
        }
        
        stage ("terraform plan") {
            steps {
                withAWS(credentials:'awscredentials') {
                      sh ('terraform plan') 
                }
            }
        }
        
        stage ("Kube check") {
            steps {
                withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECRED')]) {
                    sh 'cat $KUBECRED > ~/.kube/config'
                }
            }
        }    
        
        stage ("terraform Action") {
            steps {
                withAWS(credentials:'awscredentials') {
                echo "Terraform action is --> ${action}"
                sh ('terraform ${action} --auto-approve') 
                }
            }
        }

        stage ("Apply kubeconfig") {
            steps {
                sh ('aws eks --region $(terraform output -raw region) update-kubeconfig --name $(terraform output -raw cluster_name)')
            }
        }
    }
}