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
                sh 'echo Dizzy*22 | sudo -S'
                sh 'cd capstone/greendeploy/'                
                sh './run_docker.sh'
            }
        }
        stage('Push green image') {
            steps {
                sh 'cd capstone/greendeploy/'
                sh 'echo Dizzy*22 | sudo -S ./upload_docker.sh'
            }
        }
        stage('Create the kubeconfig file') {
            steps {
              withAWS(region: 'us-east-2', credentials: 'Jenkins'){
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