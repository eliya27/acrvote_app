pipeline {
    agent any

    environment {
        AZURE_SUBSCRIPTION_ID='b2c50cc7-e835-4e58-a990-d2c564a07420'
        AZURE_TENANT_ID='89dc1d2c-a8dd-4803-b8ef-1c8963b09b20'
        CONTAINER_REGISTRY='eliyareg2'
        RESOURCE_GROUP='AKSRG'
        REPO="azure_vote_jenkins"
        IMAGE_NAME="azure_vote_app"
        TAG="latest"
    }

    stages {
        stage('Login to Azure account') {
                            
            
            steps {
                               echo "Login to Azure cloud"
                withCredentials([usernamePassword(credentialsId: 'acr_sp2', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
                            sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
                            sh 'az account set -s $AZURE_SUBSCRIPTION_ID'
                               echo "Finish Login to Azure cloud"
                            
                        }
                             
            }
        }
        
        stage('Login to container registry') {
                             
                             
            steps {
                             echo "Login to Container registry"
                withCredentials([usernamePassword(credentialsId: 'acr_sp2', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
                            
                            sh 'az acr login --name $CONTAINER_REGISTRY --resource-group $RESOURCE_GROUP'
                             echo "Finish Login to Container registry"
                        }
                              
            }
        }
        
        stage('Build Image') {
                               
            steps {
                              echo "Start Image building"
                withCredentials([usernamePassword(credentialsId: 'acr_sp2', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
                            
                            sh 'az acr build --image $REPO/$IMAGE_NAME:$TAG --registry $CONTAINER_REGISTRY --resource-group AKSRG --file Dockerfile .' 
                              echo "Finish Image building"
                        }
            }
                               
        }
        
       stage('Container Deployment'){
                              
             steps {
                              echo "Start kubernetes deployment"
                           script{
                                kubernetesDeploy(
                                             configs: 'azure-vote-all-in-one-redis.yaml',
                                             kubeconfigId: 'K8S',
                                             enableConfigSubstitution: false
                          )
                   }
                              echo "Finish kubernetes deployment"
              }
       }
   
    }
}
