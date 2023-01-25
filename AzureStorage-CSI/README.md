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

### Terraform config

From the previous command provide the following info into providers.tf
```
{
  "appId": "",
  "displayName": "",
  "password": "",
  "tenant": ""
}
```
Provide Azure Storage config into azure_core.conf to save tftstate:
```
resource_group_name = ""
storage_account_name = ""
access_key = ""
key = ""
container_name = ""
```


Run the following commands to create AKS cluster:
```
terraform init --reconfigure --backend-config="azure_core.conf"
terraform plan -var-file="variables/core.tfvars"
terraform apply -var-file="variables/core.tfvars" -auto-approve
```
Run the following command to destroy AKS cluster:
```
terraform destroy -var-file="variables/core.tfvars" -auto-approve
```

