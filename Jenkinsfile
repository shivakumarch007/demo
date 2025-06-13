pipeline {
    agent any
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
        stage('Deploy') {
            steps {
                echo "deploy"
            }
        }
    }
}