pipeline {
    agent any

    tools {
        maven 'maven' // Use the name configured in Jenkins > Global Tool Configuration
    }

    environment {
        IMAGE_NAME = "springboot"
        IMAGE_TAG = "latest"
        ACR_NAME = "springbootdemo.azurecr.io" // Ensure this is your full ACR login server
        FULL_IMAGE = "${ACR_NAME}/${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {
        stage('Checkout From Git') {
            steps {
                git branch: 'main', url: 'https://github.com/shivakumarch007/demo.git'
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
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }
        stage('Login to ACR') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'ACR_CREDS', usernameVariable: 'ACR_USER', passwordVariable: 'ACR_PASS')
                ]) {
                    sh '''
                        echo "$ACR_PASS" | docker login $ACR_NAME -u $ACR_USER --password-stdin
                    '''
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                   echo 'Docker Push Started'
                     sh """
                     docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${FULL_IMAGE}
                     docker push ${FULL_IMAGE}
                     """
                      }
                }
        }

    }
}
