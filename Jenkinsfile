# Jenkins Pipeline for CI/CD Integration

pipeline {
   agent any
   
   environment {
       DOCKERHUB_USERNAME = credentials('dockerhub-username')
       DOCKERHUB_PASSWORD = credentials('dockerhub-password')
       IMAGE_NAME = 'tomerhanan/rolling-project-image'
   }
   
   stages {
       stage('Clone Repository') {
           steps {
               git branch: 'main', url: 'https://github.com/TomerHanan/Rolling-Project.git'
           }
       }
       
       stage('Parallel Checks') {
           parallel {
               stage('Linting') {
                   steps {
                       sh 'pip install flake8'
                       sh 'flake8 python/'
                       sh 'shellcheck python/*.sh || true'
                       sh 'hadolint demoapp/Dockerfile || true'
                   }
               }
               stage('Security Scan') {
                   steps {
                       sh 'pip install safety'
                       sh 'safety check -r python/requirements.txt || true'
                       sh 'trivy image $IMAGE_NAME || true'
                   }
               }
           }
       }
       
       stage('Build Docker Image') {
           steps {
               sh 'docker build -t $IMAGE_NAME demoapp/'
           }
       }
       
       stage('Push to Docker Hub') {
           steps {
               sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
               sh 'docker push $IMAGE_NAME'
           }
       }
   }
   
   post {
       success {
           echo 'Pipeline completed successfully!'
       }
       failure {
           echo 'Pipeline failed! Check logs for details.'
       }
   }
}