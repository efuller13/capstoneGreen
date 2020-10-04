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
               sh '''
                            #!/usr/bin/env bash

                            ## Complete the following steps to get Docker running locally

                            # Step 1:
                            # Build image and add a descriptive tag
                            docker build --tag=greenimage .

                            # Step 2: 
                            # List docker images
                            docker image ls

                            # Step 3: 
                            # Run flask app
                            docker run -p 8000:80 greenimage
                 '''
            }
        }
        stage('Push green image') {
            steps {
               sh '''
                              echo Dizzy*22 | sudo -S cd capstone/greendeploy/
                              echo Dizzy*22 | sudo -S ./upload_docker.sh
                 '''
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