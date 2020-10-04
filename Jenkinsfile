pipeline {
    agent any

    stages {
        stage('Linting') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }
        stage('Build green image') {
            steps {
                sh 'sudo -s ./run_docker.sh'
            }
        }
        stage('Push green image') {
            steps {
                sh 'sudo -s ./upload_docker.sh'
            }
        }
        stage('Create the kubeconfig file') {
            steps {
              withAWS(region: 'us-east-2', credentials: 'aws_credentials'){
               sh '''
                              aws eks --region us-east-2 update-kubeconfig --name Capstone
                 '''
              }
            }
        }
        stage('Deploy green container') {
            steps {
                sh 'kubectl apply -f ./green-controller.json'
            }
        }
        stage('Redirect service to green container') {
            steps {
                sh 'kubectl apply -f ./blue-green-service.json'
            }
        }        
    }
}