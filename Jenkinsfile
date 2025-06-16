pipeline {
    agent any

    tools {
        maven 'maven' // Use the name configured in Jenkins > Global Tool Configuration
    }

    environment {
        // Docker and ACR
        IMAGE_NAME = 'spingbootdemo'
        IMAGE_TAG = "latest"  // Auto-increments each Jenkins build
        ACR_NAME = 'spingbootdemo.azurecr.io' // Ensure this is your full ACR login server
        FULL_IMAGE_NAME = "${ACR_NAME}/${IMAGE_NAME}:${IMAGE_TAG}"
        // AKS and Azure SP
        TENANT_ID = '01ec41c1-1d77-4daf-855f-de00da327022'       // <-- Replace with your actual tenant ID
        RESOURCE_GROUP = 'Demo'                                   // <-- Your AKS resource group
        CLUSTER_NAME = 'myAKSCluster'                             // <-- Your AKS cluster name
        
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
                    docker.build("${FULL_IMAGE_NAME}")
                }
            }
        }
        stage('Login to ACR') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'ACR_CREDS', usernameVariable: 'ACR_USER', passwordVariable: 'ACR_PASS')
                ]) {
                    sh """
                        echo "$ACR_PASS" | docker login $ACR_NAME -u $ACR_USER --password-stdin
                    """
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                   echo 'Docker Push Started'
                     sh """
                     docker push ${FULL_IMAGE_NAME}
                     """
                }
            }
        }
        stage('Get AKS Credentials') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'AKS_CREDS', usernameVariable: 'AZURE_CLIENT_ID', passwordVariable: 'AZURE_CLIENT_SECRET')])
                     {
                    echo 'Getting AKS Credentials'
                     sh '''
                     az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant ${TENANT_ID}
                     az aks get-credentials --resource-group ${RESOURCE_GROUP} --name ${CLUSTER_NAME} --overwrite-existing
                     '''
                }
            }
        }
        stage('Deploy to AKS') {
            steps {
               script {
                    echo 'Deploying to AKS'
                     sh """
                     sed -i 's|image: .*|image: ${FULL_IMAGE_NAME}|' springboot-deployment.yaml
                     kubectl apply -f k8s/springboot-deployment.yaml
                     """
                }
            }
        }

    }
}
