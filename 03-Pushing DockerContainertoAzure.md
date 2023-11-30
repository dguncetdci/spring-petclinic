
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

```

### 1. Create a Kubernetes Deployment Configuration
Create a YAML file for the deployment. Here's a template:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: java-app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: java-app
  template:
    metadata:
      labels:
        app: java-app
    spec:
      containers:
      - name: java-container
        image: <YOUR_ACR_NAME>.azurecr.io/java-docker:latest
        ports:
        - containerPort: 8080
```
Replace `<YOUR_ACR_NAME>` with your Azure Container Registry name.

### 2. Apply the Deployment File
Deploy your application in AKS:

```bash
kubectl apply -f deployment.yaml
```

### 3. Expose the Deployment as a Service
To make your application accessible externally:

```bash
kubectl expose deployment java-app-deployment --type=LoadBalancer --name=java-app-service --port=8080
```

### 4. Verify Deployment
Check the status of the deployment and service:

```bash
kubectl get deployments
kubectl get services
```

### 5. Access the Application
Once an external IP is assigned, access your application at `http://<EXTERNAL_IP>:8080`.

## Additional Configurations
Depending on your application's needs, you might need additional configurations such as environment variables or volume mounts.
