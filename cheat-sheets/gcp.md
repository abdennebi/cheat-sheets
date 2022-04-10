gcloud reference : https://cloud.google.com/sdk/gcloud/reference/


## Accounts

- Get active account's email

````
gcloud auth list --filter=status:ACTIVE --format="value(account)"
````

### Set the default project

- List the projects : 

````
gcloud projects list
````
- Set the default project 

````
gcloud config set project MyProject
````

- Get project's id

````
PROJECT_ID=$(gcloud config get-value core/project)
````

- Get project's number

````
PROJECT_NUMBER=$(gcloud projects describe ${PROJECT_ID} --format='value(projectNumber)')
````

### Set a default  Compute Engine zone
Example : Set the default zone to ``us-east1-d`` :

````
gcloud config set compute/zone us-east1-d
````

### Get all available zones

````
gcloud compute zones list
````

### Ensure the default credentials are available local machine

````
gcloud auth application-default login
````

- Print access token

````
gcloud auth application-default print-access-token
````

### Enable OS Login

````
gcloud compute project-info add-metadata --metadata enable-oslogin=TRUE
````

## Container Engine

- List clusters

````
gcloud container clusters list
````

- Shutdown all nodes

````
gcloud container clusters resize MyCluster --size=0
````

- Get active cluster

````
gcloud config get-value container/cluster
````

- Generate kubeconfig file for ``MyCluster``

````
gcloud container clusters get-credentials MyCluster
````

## KMS

- Create keyring

````
gcloud kms keyrings create my-keyring --location=global
````

- Generate a new encryption key

````
gcloud kms keys create my-key \
  --location=global \
  --keyring=my-keyring \
  --purpose=encryption
````  
- Encrypt a file using the ``my-keyring`` keyring and ``my-key`` encryption key

````
gcloud kms encrypt \
  --plaintext-file my-file \
  --ciphertext-file my-file.enc \
  --location=global \
  --keyring=my-keyring \
  --key=my-key
````

## Connectivity

- Make the VM public
````
gcloud compute instances add-access-config admin-vm
````

- Remove the VM's public address
````
gcloud compute instances delete-access-config admin-vm
````

- Get the VM's external address
````
curl -s -H "Metadata-Flavor: Google"   http://metadata.google.internal/computeMetadata/v1/instance/network-interfaces/0/access-configs/0/external-ip
````

- Get other metadata
````
curl -s -H "Metadata-Flavor: Google"   http://metadata.google.internal/computeMetadata/v1/instance
````

## Deployment Manager

 - List supported resource types
 
 ````bash
 gcloud deployment-manager types list
 ````
 
 - List supported type by Google Type Provider
 
 ````bash
 gcloud beta deployment-manager types list --project gcp-types
 ````
 
 ## Security Command Center
 
 - List all ``ACTIVE`` security findings
 
 ````bash
 gcloud alpha scc findings list $ORG_ID --filter="state=\"ACTIVE\"" --field-mask="finding.category,finding.resource_name"
 
 ````
 
 ## Cloud Asset Inventory
 
  ````bash
 ````
 
 ## Audit Logs
 
 To read your Google Cloud project-level audit log entries, run the following command:

  ````bash
  gcloud logging read "logName : projects/$PROJECT_ID/logs/cloudaudit.googleapis.com" --project=$PROJECT_ID
 ````
 
 To read your folder-level audit log entries:

````bash
 gcloud logging read "logName : folders/$FOLDER_ID/logs/cloudaudit.googleapis.com" --folder=$FOLDER_ID
 ````
 
 To read your organization-level audit log entries:
 
 ````bash
 gcloud logging read "logName : organizations/$ORG_ID/logs/cloudaudit.googleapis.com" --organization=$ORG_ID
 ````
 
## Log Sinks

````bash
gcloud logging sinks create gcp_logging_sink_pubsub pubsub.googleapis.com/projects/$PROJECT_ID/topics/logs-export-topic  --include-children --organization=$ORG_ID
````

## Customer Managed Encryption Key CMEK Google Service Agents

- Storage : ``service-${PROJECT_NUMBER}@gs-project-accounts.iam.gserviceaccount.com``

