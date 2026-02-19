# project_tf_S3

## terraform.tfvars example

```hcl
s3_bucket_name = "hybrid-video-minio-backup-prod"
project_name = "hybrid-video"
aws_region = "ap-northeast-2"
existing_iam_user_name = "user05"
  -> iam 사용자는 본인이 사용할 계정명을 입력하면 됩니다.
