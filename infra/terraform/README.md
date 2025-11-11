# Terraform remote backend
Before running terraform init, create:
- S3 bucket: <your-terraform-state-bucket> with encryption enabled
- DynamoDB table: <your-terraform-locks-table> (Partition key: LockID) for state locking
