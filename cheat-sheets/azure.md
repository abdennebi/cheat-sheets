- List authenticated accounts

```
az account list
```

- Find guest users

  ````
  az ad user list --query "[?userType=='Guest']"
  `````
  
  ````PoweShell
  Get-AzureADUser |Where-Object {$_.UserType -like "Guest"} |Select-Object DisplayName, UserPrincipalName, UserType -Unique
  ````
- List roles

  ````
  az role definition list | grep roleName
  ````
## Security Center

- Check Security Center pricing tier
  ````
  az account get-access-token --query "{subscription:subscription,accessToken:accessToken}" --out tsv | xargs -L1 bash -c 'curl -X GET -H "Authorization: Bearer $1" -H "Content-Type: application/json" https://management.azure.com/subscriptions/$0/providers/Microsoft.Security/pricings?api-version=2018-06-01' | jq '.|.value[] | select(.name=="VirtualMachines")'|jq '.properties.pricingTier'
  ````
  
## Storage

- List storage accounts
  ````
  az storage account list --query [*].[name,enableHttpsTrafficOnly]
  ````

## Azrue Info

- Get Tenant Id

  ````
  Get-AzTenant
  ````
