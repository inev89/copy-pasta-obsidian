# Azure CLI Help Messages
```
az help | less
```

# Accounts
## Look at credentials

```
az account show | less 
```

## List all accounts
```
az account list --query "[].name"
```

```
az account list --query "[?state=='Enabled'].{Name:name, ID:id}"
```

```
az role assignment list --scope "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64" --query [?roleDefinition=='Owner']

```

## Group Members

```
az ad member list
```

# Resource Groups
## List all resource group
```bash
az group list -o table
```

# Storage
## List all storage accounts

```
az storage account list | less
```

## List Storage accounts associated with a resource group

```bash
az storage account list --resource-group rg-the-neighborhood -o table
```

## Show storage accounts

```
az storage account show --name neighborhood2 | less
```

```
az storage container list --account-name neighborhood2 --auth-mode login
```


## Look into Storage Blobs

```
az storage blob service-properties show --account-name neighborhoodhoa --auth-mode login
```

```
az storage blob list --account-name neighborhood2 --container-name public --auth-mode login
```


## View blob to console

```
az storage blob download --container-name '$web' --account-name neighborhoodhoa --auth-mode login --name 'iac/terraform.tfvars' --file /dev/stdout
```

```
az storage blob download --container-name 'public' --account-name neighborhood2 --auth-mode login --name 'admin_credentials.txt' --file /dev/stdout | less
```



# Network Security Groups

## List Groups

```
az network nsg list -o table
```

## Show details

```
az network nsg show --name nsg-web-eastus --resource-group theneighborhood-rg1
```

## List Rules of a NSG

```
az network nsg rule list --name nsg-mgmt-eastus --resource-group theneighborhood-rg2
```


```
az network nsg rule list --name nsg-mgmt-eastus --resource-group theneighborhood-rg2
```


## Show rules

```
az network nsg rule show --nsg-name nsg-production-eastus --resource-group theneighborhood-rg1 --name "Allow-RDP-From-Internet"
```


Your mission: Find a way to bypass the current restrictions and elevate to 
fire safety admin privileges. Once you regain full access, run the special 
command `/etc/firealarm/restore_fire_alarm` to restore complete fire alarm system control and 
protect the Dosis neighborhood from potential emergencies.


User chiuser may run the following commands on bb857d4cb228:
    (root) NOPASSWD: /usr/local/bin/system_status.sh

/usr/local/bin/system_status.sh


🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨
              DOSIS NEIGHBORHOOD FIRE ALARM SYSTEM - LOCKOUT MODE
🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨🔥🚨

🚨 EMERGENCY ALERT: Fire alarm system admin access has been compromised! 🚨
The fire safety systems are experiencing interference and 
admin privileges have been mysteriously revoked. The neighborhood's fire 
protection infrastructure is at risk!

⚠️ CURRENT STATUS: Limited to standard user access only
🔒 FIRE SAFETY SYSTEMS: Partially operational but restricted
🎯 MISSION CRITICAL: Restore full fire alarm system control

Your mission: Find a way to bypass the current restrictions and elevate to 
fire safety admin privileges. Once you regain full access, run the special 
command `/etc/firealarm/restore_fire_alarm` to restore complete fire alarm system control and 
protect the Dosis neighborhood from potential emergencies.