pipeline {
    agent any
    tools {
        maven 'maven' // Replace 'M3' with the name configured in Jenkins > Global Tool Configuration
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
        }
    }
}