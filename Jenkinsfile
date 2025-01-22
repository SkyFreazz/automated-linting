 pipeline {
     agent any
     stages {
         stage('install'){
             steps {
                 sh '''
                 #!/bin/bash
                 apt install python3.10-venv
                 python3 -m venv venv
                 . ./venv/bin/activate
                 pip install flake8
                 flake8 app.py
                 '''
             }
         }
         stage('Lint') {
             steps {
                 sh 'flake8 app.py'
             }
         }
         stage('Format') {
             steps {
                 sh 'black --check app.py'
             }
         }
         stage('Build') {
             steps {
                 sh 'docker build -t <dockerhub-username>/<repo-name>:0.0.1 .'
                 sh 'docker push <dockerhub-username>/<repo-name>:0.0.1'
             }
         }
     }
 }
