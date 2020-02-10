# Apache-ACR
Set up a Docker based container in Azure and install Apache

## Create docker images
1) To create Build from Dockerfile you should in same level where Docker file located on my Repo.

    `docker build -t ankur-devops-apache-az .`
2) To check and run locally

    `docker run -p 8080:80 ankur-devops-apache-az:latest`
    
    so it will run your project on http://localhost:8080
    

## Steps to create follow create azure container and deploy on it.

1) Create a resource group with the az group create command with "ankurResourceGrp" and name "ankurdevops"
    
    `az acr create --resource-group ankurResourceGrp --name ankurdevops --sku Basic`

2) Log in to container registry
    
    `az acr login --name ankurdevops`

3) Get the full login server name for your Azure container registry
    
    `az acr show --name ankurdevops --query loginServer --output table`

4) Tagging the local container to Azuse container Registry 
    
    `docker tag ankur-devops-apache-az ankurdevops.azurecr.io/ankur-devops-apache-az:v1`

5) Deploying to ACR
    
    `docker push ankurdevops.azurecr.io/ankur-devops-apache-az:v1`

6) List images in Azure Container Registry
    
    `az acr repository list --name ankurdevops --output table`

7) List repository tag from Azure Container Registry
    
    `az acr repository show-tags --name ankurdevops --repository ankur-devops-apache-az --output table`

8) Get full name of the container registry login server 
    
    `az acr show --name ankur-devops-apache-az --query loginServer`

9) Use the az container create command to deploy the container
    
    `az container create --resource-group ankurResourceGrp --name ankur-devops-apache-az --image ankurdevops.azurecr.io/ankur-devops-apache-az:v1 --cpu 1 --memory 1 --registry-login-server ankurdevops.azurecr.io --registry-username <USER> --registry-password <PASSWORD> --dns-name-label ankur-devops-apache-az-dns --ports 80`


10) Verify deployment progress
    
    `az container show --resource-group ankurResourceGrp --name ankur-devops-apache-az --query instanceView.state`

11) View the application and container logs
    
    `az container show --resource-group ankurResourceGrp --name ankur-devops-apache-az --query ipAddress.fqdn`
