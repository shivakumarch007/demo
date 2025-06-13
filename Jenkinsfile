pipeline {
    agent any
    stages {
        stage('Checkout From Git') {
            steps {
                git branch : 'main' , url : 'https://github.com/shivakumarch007/demo.git'
            }
        }
        stage('Test') {
            steps {
                echo "test"
            }
        }
        stage('Deploy') {
            steps {
                echo "deploy"
            }
        }
    }
}