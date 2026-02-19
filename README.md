# project_tf_S3

Terraform을 사용하여 AWS S3 버킷을 생성하는 모듈

본 S3는 영상 원본 저장소(Primary Storage)로 사용되며, 온프레미스 MinIO 환경에서 해당 S3 버킷을 mirror 대상으로 구성


## 📌 Architecture Overview

```bash
AWS S3 (Primary Storage)
      ↓ Mirror
On-Prem MinIO (Secondary / Replica)
```
- S3에 업로드된 영상 데이터를 MinIO의 mirror 기능을 통해 동기화


## 📁 Project Structure

```bash
s3/
├─ iam.tf
├─ s3.tf
├─ outputs.tf
├─ variables.tf
├─ terraform.tfvars
└─ .gitignore
```


## ⚙ What This Module Creates

- S3 Bucket
- Versioning 설정
- Public Access 차단
- IAM 사용자 권한 연결
- 태그 기반 리소스 관리


## 📋 terraform.tfvars example

```hcl
s3_bucket_name = "영상 원본이 저장될 S3 버킷 이름"
project_name = "태그에 사용될 프로젝트 명"
aws_region = "S3 리전"
existing_iam_user_name = "S3 접근 권한을 부여할 IAM 사용자"
  -> iam 사용자는 본인이 사용할 계정명을 입력하면 된다.
```


## 🔐 Security Notes

- S3는 Private Bucket으로 구성
- Public Access는 차단
- IAM 최소 권한 정책 적용 권장
- Access Key는 Git에 업로드하지 않는다.


## ⚠ Important

- Versioning이 활성화된 경우 객체 삭제 시 모든 버전 관리에 주의
- 운영 환경에서는 Server-Side Encryption(KMS) 적용 권장

```bash
s3.tf

resource "aws_s3_bucket" "minio_backup" {
  ...
  force_destroy = true
  ...
```
현재 코드는 위 명령을 추가하여 `terraform destroy` 시 버킷 내부 객체 여부에 상관없이 지워지도록 구성되어 있다.
