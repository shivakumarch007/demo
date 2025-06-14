pipeline {
    agent any
    tools {
        maven 'maven' // Replace 'M3' with the name configured in Jenkins > Global Tool Configuration
    }
    environment {
        IMAGE_NAME = "springboot"
        IMAGE_TAG =  "latest"
        ACR_NAME = 'springbootacr'  // Replace with your ACR login server
        IMAGE_NAME = 'myapp:latest'
        FULL_IMAGE = "${ACR_NAME}/${IMAGE_NAME}:${IMAGE_TAG}"
    }
    stages {
        stage('Checkout From Git') {
            steps {
                git branch : 'main' , url : 'https://github.com/shivakumarch007/demo.git'
            }
        }
        stage('Maven Package') {
            steps {
                echo 'This is Maven Package Stage'
                sh 'mvn package'
            }
        } 
        stage('Docker Build') {
            steps {
                script {
                     echo 'Docker Build Started'
                     docker.build ("$IMAGE_NAME:$IMAGE_TAG")
                }
            }
        stage('Log into ACR') {
            steps {
                withCredentials([
                    string(credentialsId: '	AZURE_USERNAME', variable: 'AZURE_USERNAME'),
                    string(credentialsId: 'AZURE_PASSWORD', variable: 'AZURE_PASSWORD')
                ]) {
                sh '''
                  echo "Logging into ACR..."
                  echo "$ACR_PASS" | docker login $ACR_NAME -u $ACR_USER --password-stdin
                  '''
                   }
                }
            }
        }
    }
}