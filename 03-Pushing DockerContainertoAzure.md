
# Pushing Docker Container to Azure

## Defining Local Variables
Set the following environment variables in your local environment:

```bash
export AZ_RESOURCE_GROUP=javacontainerizationdemorg
export AZ_CONTAINER_REGISTRY=<YOUR_CONTAINER_REGISTRY>
export AZ_KUBERNETES_CLUSTER=javacontainerizationdemoaks
export AZ_LOCATION=<YOUR_AZURE_REGION>
export AZ_KUBERNETES_CLUSTER_DNS_PREFIX=<YOUR_UNIQUE_DNS_PREFIX_TO_ACCESS_YOUR_AKS_CLUSTER>
```

## Pushing Container from Local Docker to Azure
Follow these steps to push your Docker container to Azure Container Registry (ACR) and deploy it to Azure Kubernetes Service (AKS).

### 1. Log in to Azure
```bash
az login
```

### 2. Tag Your Docker Image
Replace `<image_name>` with the name of your local Docker image.
```bash
docker tag <image_name> $AZ_CONTAINER_REGISTRY.azurecr.io/<image_name>:v1
```

### 3. Log in to Azure Container Registry
```bash
az acr login --name $AZ_CONTAINER_REGISTRY
```

### 4. Push the Image to ACR
```bash
docker push $AZ_CONTAINER_REGISTRY.azurecr.io/<image_name>:v1
```

### 5. Deploy the Image to AKS
```bash
az aks create --resource-group $AZ_RESOURCE_GROUP --name $AZ_KUBERNETES_CLUSTER --node-count 1 --enable-addons monitoring --generate-ssh-keys --location $AZ_LOCATION --dns-name-prefix $AZ_KUBERNETES_CLUSTER_DNS_PREFIX
az aks get-credentials --resource-group $AZ_RESOURCE_GROUP --name $AZ_KUBERNETES_CLUSTER
kubectl run <app_name> --image=$AZ_CONTAINER_REGISTRY.azurecr.io/<image_name>:v1
```

## Pushing Container from Docker Online to Azure
For pushing from Docker Online, ensure you're logged into Docker and have access to your image repository.

### 1. Tag and Push the Image from Docker Online
Repeat the steps above for tagging and pushing your Docker image.

### 2. Deploy the Image to AKS from Docker Online
Use the same AKS deployment commands as mentioned in the previous section.

Replace `<image_name>` and `<app_name>` with your specific image and application names.
