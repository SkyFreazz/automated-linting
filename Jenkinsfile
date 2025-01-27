 pipeline {
     agent any
     environment {
         DOCKER_CREDENTIALS = credentials('e0f79c37-13a6-4fa7-b95f-ebd046877ad0') // Use the ID of your credentials
         KUBECONFIG = '/var/lib/jenkins/.kube/config'
     }
     stages {
         stage('Lint') {
             steps {
                 sh '''
                 #!/bin/bash
                 python3 -m venv venv
                 . ./venv/bin/activate
                 pip install flake8
                 flake8 app.py
                 '''
             }
         }
         stage('Format') {
             steps {
                 sh '''
                 #!/bin/bash
                 python3 -m venv venv
                 . ./venv/bin/activate
                 pip install black
                 black --check app.py
                 '''
             }
         }
         stage('Build') {
             steps {
		 sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin'
                 sh 'docker build -t skyfreazz/flask-app:0.0.1 .'
                 sh 'docker push skyfreazz/flask-app:0.0.1'
             }
         }
         stage('Deploy to Kubernetes') {
             steps {
                 sh 'kind get cluster --name lab_jenkins'
                 sh 'kubectl --kubeconfig=$KUBECONFIG apply -f deployment.yaml'
                 sh 'kubectl --kubeconfig=$KUBECONFIG apply -f service.yaml'
             }
         }
     }
 }
