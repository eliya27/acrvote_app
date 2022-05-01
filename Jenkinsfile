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
        stage('container registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'acr_sp2', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
                            sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
                            sh 'az account set -s $AZURE_SUBSCRIPTION_ID'
                            sh 'az acr login --name $CONTAINER_REGISTRY --resource-group $RESOURCE_GROUP'
                            sh 'az acr build --image $REPO/$IMAGE_NAME:$TAG --registry $CONTAINER_REGISTRY --resource-group AKSRG --file Dockerfile . '
                        }
            }
        }
    }
}
