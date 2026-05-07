pipeline {
    agent any

    environment {
        RG = 'flask-aks-rg-sea'
        AKS = 'flask-aks-cluster'
        ACR_NAME = 'flaskelli1'
        ACR_LOGIN_SERVER = 'flaskelli1.azurecr.io'
        IMAGE_NAME = 'flask-app'
        NAMESPACE = 'flask-demo'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Azure Login') {
            steps {
                withCredentials([string(credentialsId: 'azure-sp-json', variable: 'AZURE_CREDENTIALS')]) {
                    sh '''
                    echo "$AZURE_CREDENTIALS" > azure-sp.json

                    CLIENT_ID=$(python3 -c "import json; print(json.load(open('azure-sp.json'))['clientId'])")
                    CLIENT_SECRET=$(python3 -c "import json; print(json.load(open('azure-sp.json'))['clientSecret'])")
                    TENANT_ID=$(python3 -c "import json; print(json.load(open('azure-sp.json'))['tenantId'])")

                    az login --service-principal \
                      -u "$CLIENT_ID" \
                      -p "$CLIENT_SECRET" \
                      --tenant "$TENANT_ID"
                    '''
                }
            }
        }

        stage('Build and Push Image') {
            steps {
                sh '''
                az acr login --name $ACR_NAME

                docker buildx build \
                  --platform linux/amd64 \
                  -t $ACR_LOGIN_SERVER/$IMAGE_NAME:$IMAGE_TAG \
                  --push .
                '''
            }
        }

        stage('Deploy to AKS') {
            steps {
                sh '''
                az aks get-credentials \
                  --resource-group $RG \
                  --name $AKS \
                  --overwrite-existing

                kubectl create namespace $NAMESPACE --dry-run=client -o yaml | kubectl apply -f -

                sed "s|IMAGE_PLACEHOLDER|$ACR_LOGIN_SERVER/$IMAGE_NAME:$IMAGE_TAG|g" k8s/deployment.yaml > k8s/deployment.generated.yaml

                kubectl apply -f k8s/deployment.generated.yaml
                kubectl apply -f k8s/service.yaml

                kubectl rollout status deployment/flask-app -n $NAMESPACE
                '''
            }
        }
    }

    post {
        always {
            sh '''
            rm -f azure-sp.json
            '''
        }
    }
}
