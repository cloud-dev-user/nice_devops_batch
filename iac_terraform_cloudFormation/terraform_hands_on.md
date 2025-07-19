# Simple Terraform Project – S3 Bucket with Module & Conditionals

---

## Goal

Provision an S3 bucket using a Terraform module with optional versioning (conditional resource).

---

### Step 1: Create Module

**Directory structure:**

```
terraform-simple-s3/
├── main.tf
└── modules/
    └── s3_bucket/
        ├── main.tf
        └── variables.tf
```

---

### Step 2: Module code (`modules/s3_bucket/main.tf`)

```hcl
resource "aws_s3_bucket" "this" {
  bucket = var.bucket_name
  acl    = "private"

  versioning {
    enabled = var.versioning_enabled
  }
}
```

---

### Step 3: Module variables (`modules/s3_bucket/variables.tf`)

```hcl
variable "bucket_name" {
  type = string
}

variable "versioning_enabled" {
  type    = bool
  default = false
}
```

---

### Step 4: Root `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

variable "enable_versioning" {
  type    = bool
  default = true
}

module "s3_bucket" {
  source             = "./modules/s3_bucket"
  bucket_name        = "your-unique-bucket-name-12345"
  versioning_enabled = var.enable_versioning
}
```

---

### Step 5: Initialize Terraform

```bash
terraform init
```

---

### Step 6: Preview and Apply

```bash
terraform plan
terraform apply
```

Confirm by typing `yes`.

---

### Step 7: Change Conditional

To disable versioning:

```bash
terraform apply -var="enable_versioning=false"
```

---

### Step 8: Clean Up

```bash
terraform destroy
```
