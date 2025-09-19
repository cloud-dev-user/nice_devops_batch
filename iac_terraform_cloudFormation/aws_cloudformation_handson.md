Sure! Here's a **simpler real-project style hands-on guide** for CloudFormation and Terraform focusing on **just creating an S3 bucket** with validations, stack policies (CloudFormation), modules, and conditionals (Terraform). No VPC involved.

---

# Hands-On Lab 1: Simple AWS CloudFormation Project – S3 Bucket

---

## Goal

Deploy an S3 bucket with:

* Bucket versioning enabled
* Stack policy protecting the bucket from accidental deletion

---

### Step 1: Create CloudFormation Template `s3-bucket.yaml`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Simple S3 Bucket with Versioning

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: your-unique-bucket-name-12345
      VersioningConfiguration:
        Status: Enabled

Outputs:
  BucketName:
    Description: "S3 Bucket Name"
    Value: !Ref MyS3Bucket
```

---

### Step 2: Validate Template

```bash
aws cloudformation validate-template --template-body file://s3-bucket.yaml
```

---

### Step 3: Create Stack

```bash
aws cloudformation create-stack --stack-name SimpleS3Stack --template-body file://s3-bucket.yaml
```

---

### Step 4: Add Stack Policy to Protect Bucket from Deletion

Create `stack-policy.json`:

```json
{
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "Update:Delete",
      "Principal": "*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ResourceType": "AWS::S3::Bucket"
          "LogicalResourceId": "MyS3Bucket"
        }
      }
    }
  ]
}
```

Attach it:

```bash
aws cloudformation set-stack-policy --stack-name SimpleS3Stack --stack-policy-body file://stack-policy.json
```

---

### Step 5: Update Stack (Try changing versioning or bucket name)

Use:

```bash
aws cloudformation update-stack --stack-name SimpleS3Stack --template-body file://s3-bucket.yaml
```

The stack policy will prevent bucket deletion.

---

### Step 6: Delete Stack (Remove stack policy first or fail)

```bash
aws cloudformation set-stack-policy --stack-name SimpleS3Stack --stack-policy-body '{}'

aws cloudformation delete-stack --stack-name SimpleS3Stack
```

