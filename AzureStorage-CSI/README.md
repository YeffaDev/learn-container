### Link
https://learn.microsoft.com/en-us/azure/developer/terraform/create-k8s-cluster-with-tf-and-aks

https://learn.microsoft.com/en-us/azure/aks/azure-files-csi

### Install Azure CLI

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

### Install terraform 

```
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

### Create SP

```
az login
az ad sp create-for-rbac --name <service_principal_name> --role Contributor --scopes /subscriptions/<subscription_id>
```

### From the previous command provide the following info into providers.tf
```
{
  "appId": "",
  "displayName": "",
  "password": "",
  "tenant": ""
}
```

### Create App registration and copy objectId
```
az ad app create --display-name aks-app
```

### Create secret and paste objectId
```
az ad app credential reset --id <object_id> --append
```

### Provide appid and secrets from the previous command to core.tfvars:
```
aks_service_principal_app_id = "<app_id>"
aks_service_principal_client_secret = "<app_secret>"
```

### Provide Azure Storage config into azure_core.conf to save tftstate:
```
resource_group_name = ""
storage_account_name = ""
access_key = ""
key = ""
container_name = ""
```

### Run the following commands to create AKS cluster:
```
terraform init --reconfigure --backend-config="azure_core.conf"
terraform plan -var-file="variables/core.tfvars"
terraform apply -var-file="variables/core.tfvars" -auto-approve
```

### Check node
```
kubectl get node
```

### Check Storage Class
```
kubectl get storageclass
```

### Create pvc 
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azurefile-csi-driver/master/deploy/example/pvc-azurefile-csi.yaml
kubectl get pvc
```

### Create a pod and attach a pvc
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azurefile-csi-driver/master/deploy/example/nginx-pod-azurefile.yaml
```

### Check volume mounted in a pod
```
kubectl exec nginx-azurefile -- ls -l /mnt/azurefile
```

### Run the following command to destroy AKS cluster:
```
terraform destroy -var-file="variables/core.tfvars" -auto-approve
```
