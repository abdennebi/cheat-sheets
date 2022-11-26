IAM members which can impersonate a service account?

```
gcloud asset analyze-iam-policy  --organization='$ID' --permissions="iam.serviceAccounts.actAs" \
    --format="flattened(ACLs[0].resources[0].fullResourceName,policy.binding.members,policy.binding.role)"
```

````
List SA Keys

NOW=$(TZ=GMT date +"%Y-%m-%dT%H:%M:%SZ")
gcloud asset list --project='PROJECT_ID' \
  --billing-project='BILLING_PROJECT_ID' \
  --asset-types='compute.googleapis.com/Instance' \
  --snapshot-time=$NOW \
  --content-type='resource'


iam.googleapis.com/ServiceAccountKey

````
