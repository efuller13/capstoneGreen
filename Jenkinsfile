pipeline {
    agent any

    stages {
        stage('Linting') {
            steps {
                echo 'Building..'
            }
        }
        stage('Build green image') {
            steps {
                sh'./run_docker.sh'
            }
        }
        stage('Push green image') {
            steps {
                echo 'Deploying....'
            }
        }
        stage('Create the kubeconfig file') {
            steps {
                echo 'Deploying....'
            }
        }
        stage('Deploy green container') {
            steps {
                echo 'Deploying....'
            }
        }
        stage('Redirect service to green container') {
            steps {
                echo 'Deploying....'
            }
        }        
    }
}