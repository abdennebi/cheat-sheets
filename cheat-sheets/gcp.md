gcloud reference : https://cloud.google.com/sdk/gcloud/reference/


## Accounts

- Get active account's email

````
gcloud auth list --filter=status:ACTIVE --format="value(account)"
````


- Set ``Quota`` project which can be used by Google client libraries for billing and quota.

````bash
gcloud auth application-default set-quota-project mab-admin

# Credentials saved to file: [/Users/abdennebi/.config/gcloud/application_default_credentials.json]

# These credentials will be used by any library that requests Application Default Credentials (ADC).

# Quota project "mab-admin" was added to ADC which can be used by Google client libraries for billing and quota. Note that some services may still bill the project owning the resource.

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

 - [Search all Cloud resources](https://cloud.google.com/sdk/gcloud/reference/asset/search-all-resources) within the specified accessible scope, such as a project, folder or organization
 
  ````bash
  gcloud asset search-all-resources
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

## Generate a CSEK key

````bash
#  RFC 4648 section 4 base64-encoded AES256
openssl rand 32 > mykey.txt
openssl base64 -in mykey.txt
````

````
gcloud beta asset analyze-iam-policy --organization=ORG_ID
 --identity=SPECIFIED_USER
 --permissions=SPECIFIED_COMMA_SEPARATED_PERMISSIONS


gcloud beta asset search-all-iam-policies --query "resource: //storage.googleapis.com/*"


gcloud beta asset search-all-resources --asset-types=storage.googleapis.com/Bucket



gcloud beta asset feeds create iam-policy-feed --project=cns-security --content-type=iam-policy --pubsub-topic=projects/cns-security/topics/iam-policy-feed --asset-names="//cloudresourcemanager.googleapis.com/projects/92461541655" 



gcloud pubsub topics create org-policy-feed --project cns-security

gcloud pubsub subscriptions create org-policy-feed-test-subscription --topic org-policy-feed  --project cns-security



gcloud compute networks vpc-access connectors 


  gcloud asset search-all-iam-policies \
  --scope=projects/cns-security \
  --query='(resource : ("cloudresourcemanager" AND "projects")) AND (policy : "allusers	")' \
  --page-size=50 \
  --flatten=policy.bindings[] \
  --format='table(policy.bindings.role)'


  gcloud asset search-all-iam-policies \
  --scope=organizations/$ORG_ID \
  --query='(resource : "cloudresourcemanager") AND (policy.role.permissions : "*storage.buckets.*")' \
  --page-size=50 \
  --flatten=policy.bindings[].members[] \
  --format='table(policy.bindings.members)'


gcloud asset get-history --content-type=iam-policy --start-time=2020-06-17T21:06:01+00:00  --project cns-security  --asset-names="//cloudresourcemanager.googleapis.com/projects/92461541655"




gcloud beta asset search-all-resources --asset-types compute.googleapis.com/Instance --scope=organizations/855756567906 --project cns-security --query='name :(forseti)'


 gcloud beta asset search-all-resources --asset-types compute.googleapis.com/Instance   --scope=projects/cns-security --project cns-security

 gcloud asset search-all-resources --asset-types storage.googleapis.com/Bucket   --scope=projects/cns-security --project cns-security







gcloud beta asset search-all-iam-policies \
  --scope='organizations/855756567906' \
  --query='policy : (roles/owner}'




gcloud beta asset search-all-iam-policies   --scope='organizations/855756567906'   --query='policy : "roles/owner"'

gcloud beta asset search-all-iam-policies   --scope='organizations/855756567906'   --query='policy : "gmail.com"'

gcloud beta asset search-all-iam-policies   --scope='organizations/855756567906'   --query='policy : "allUsers"'

gcloud asset export \
   --content-type resource \
   --project cns-security \
   --output-path "gs://scc-assets-export-cns-security/assets"


gcloud asset get-history --project='PROJECT_ID' \
  --asset-names='//compute.googleapis.com/projects/test-project/zo\
nes/us-central1-f/instances/instance1' \
  --start-time='2018-10-02T15:01:23.045Z' \
  --end-time='2018-12-05T13:01:21.045Z' --content-type='resource'


Liste des resources supportées :

https://cloud.google.com/asset-inventory/docs/supported-asset-types#searchable_asset_types

https://cloud.google.com/sdk/gcloud/reference/asset

gcloud alpha scc assets list  478735633679             --filter "security_center_properties.resource_type=\"google.compute.Instance\" AND \
            resourceProperties.creationTimestamp<\"2020-05-06\""             --order-by="securityCenterProperties.resourceName ASC"   | grep creationTimestamp | wc -l

yq '{ project: .asset.securityCenterProperties.resourceProjectDisplayName,  
creationTimestamp: .asset.resourceProperties.creationTimestamp,
owners: .asset.securityCenterProperties.resourceOwners,
name: .asset.resourceProperties.name}' instances.yaml



'{ project: .asset.securityCenterProperties.resourceProjectDisplayName,
   creationTimestamp: .asset.resourceProperties.creationTimestamp,
   owners: .asset.resourceProperties.resourceOwners,
   name: .asset.resourceProperties.resourceParentDisplayName}'



jq -r '(["PROJECT","TIMESTAMP","NAME", "OWNERS"] | (., map(length*"-"))), (.[] | [.project, .creationTimestamp, .name, .owners]) | @tsv'

  jq -r '[.[]| with_entries( .key |= ascii_downcase ) ]
      |    (.[0] |keys_unsorted | @tsv)
         , (.[]|.|map(.) |@tsv)' instances.json

gcloud alpha resources list --filter="@type:compute.instances name:(rta)" --uri 

gcloud alpha resources list --filter="@type:compute.instances id:(1022799098856440546)" --uri 


status=\"TERMINATED\"

status=\"TERMINATED\"

gcloud alpha resources list --uri \
            --filter="@type:compute.instances name:(rta)"


        $ gcloud projects list \
            --format="table(projectNumber,projectId,createTime.date(tz=LOCAL\
        ))" --filter="createTime>=2018-01-15T12:00:00" --sort-by=createTime


gcloud alpha resources list --filter "@type:compute.instances AND -createTime<2020-05-06T00:00:00"

gcloud compute instances list --filter="labels.my-label:*"

gcloud compute instances list --filter="labels.my-label:*"


gcloud projects list \
            --format="table(projectNumber,projectId,createTime)" \
            --filter="createTime>=2018-01-15"



gcloud alpha resources list --filter "@type:compute.instances labels.env:prod machineType:n1-standard-1"

gcloud alpha resources list --filter "@type:compute.instances  AND machineType: \"n1-standard-16\""

gcloud alpha resources list --filter "@type:compute.instances  AND status=\"TERMINATED\" "




gcloud alpha scc assets list  --limit 5 478735633679 \
            --filter "security_center_properties.resource_type=\"google.compute.Instance\" AND \
            resourceProperties.creationTimestamp=\"2020-07-19T18:29:13.325-07:00\"" 


gcloud alpha scc assets list  --limit 5 478735633679 \
            --filter "security_center_properties.resource_type=\"google.compute.Instance\" AND \
            resourceProperties.creationTimestamp<\"2020-05-05\"" \
            --order-by="resourceProperties.name ASC" \      


"status:TERMINATED"


canIpForward: false



 https://cloudresourcesearch.googleapis.com/v1/resources:search?query=%40type%3AInstance+AND+status%3DTERMINATED&alt=json&



'@type': type.googleapis.com/compute.Instance
canIpForward: false
cpuPlatform: Intel Broadwell
creationTimestamp: '2020-07-20T02:16:24.913-07:00'
deletionProtection: false
description: ''
disk:
- autoDelete: true
  boot: true
  deviceName: persistent-disk-0
  diskSizeGb: '10'
  guestOsFeature:
  - type: VIRTIO_SCSI_MULTIQUEUE
  index: 0
  interface: SCSI
  license:
  - https://www.googleapis.com/compute/beta/projects/debian-cloud/global/licenses/debian-9-stretch
  mode: READ_WRITE
  source: https://www.googleapis.com/compute/beta/projects/natgrid-prex/zones/europe-west2-c/disks/gcp-prex-euw2c-16-1595236502-48
  type: PERSISTENT
eraseWindowsVssSignature: false
fingerprint: yfXBHmfKdsA=
guestAccelerator: []
hostname: ''
id: '8393295847310810631'
labelFingerprint: 42WmSpB8rSM=
labels: {}
machineType: https://www.googleapis.com/compute/beta/projects/natgrid-prex/zones/europe-west2-c/machineTypes/n1-standard-16
minCpuPlatform: ''
name: gcp-prex-euw2c-16-1595236502-48
networkInterface:
- accessConfig: []
  aliasIpRange: []
  fingerprint: T6fBzAHJxMQ=
  ipAddress: 10.80.9.68
  ipv6Address: ''
  name: nic0
  network: https://www.googleapis.com/compute/beta/projects/natgrid2/global/networks/nxs-network
  subnetwork: https://www.googleapis.com/compute/beta/projects/natgrid2/regions/europe-west2/subnetworks/nxs-eu-west2
privateIpv6GoogleAccess: UNSPECIFIED_INSTANCE_PRIVATE_IPV6_GOOGLE_ACCESS
resourcePolicy: []
scheduling:
  automaticRestart: false
  minNodeCpus: 0
  nodeAffinity: []
  onHostMaintenance: TERMINATE
  preemptible: true
selfLink: https://www.googleapis.com/compute/beta/projects/natgrid-prex/zones/europe-west2-c/instances/gcp-prex-euw2c-16-1595236502-48
serviceAccount:
- email: sa-fw-prex@natgrid-prex.iam.gserviceaccount.com
  scope:
  - https://www.googleapis.com/auth/cloud.useraccounts.readonly
  - https://www.googleapis.com/auth/devstorage.read_write
  - https://www.googleapis.com/auth/logging.write
  - https://www.googleapis.com/auth/monitoring.write
  - https://www.googleapis.com/auth/pubsub
  - https://www.googleapis.com/auth/service.management.readonly
  - https://www.googleapis.com/auth/servicecontrol
  - https://www.googleapis.com/auth/trace.append
sourceMachineImage: ''
startRestricted: false
status: RUNNING
statusMessage: ''
tags:
  fingerprint: SY5woybx46g=
  tag:
  - eu-grid
  - eu-grid2
zone: https://www.googleapis.com/compute/beta/projects/natgrid-prex/zones/europe-west2-c

````