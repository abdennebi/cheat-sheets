Review the S3 policy applied to bucket

````
aws s3api get-bucket-policy --bucket mwillo-backup --output text | jq
````

Update S3 IAM policy

````
aws s3api put-bucket-policy --bucket mwillo-backup --policy file://policy.json
````
