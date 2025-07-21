````markdown
# 🚀 AWS CloudFront Hands-on Guide

---

## ✅ Objective

Deliver static content (HTML, CSS, images) globally with low latency using **S3 + CloudFront**.

---

## 📦 Step-by-Step Instructions

---

### 🔹 Step 1: Create an S3 Bucket

1. Go to the [S3 console](https://console.aws.amazon.com/s3).
2. Click **Create bucket**.
3. Name it uniquely, e.g., `my-cloudfront-demo-bucket`.
4. Choose region: `us-east-1` (recommended).
5. **Uncheck** "Block all public access".
6. Confirm and click **Create bucket**.

---

### 🔹 Step 2: Upload Website Files

1. Open your bucket.
2. Click **Upload** → Add files (e.g., `index.html`).  Download hellworld.zip and extract & upload all files in it. 
3. Click **Upload**.

---

### 🔹 Step 3: Make the Files Public ( Optional if Bukcet is not made public) 

Option A: Use object actions  
- Select `index.html` → Actions → **Make public**.

Option B: Use a bucket policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-cloudfront-demo-bucket/*"
    }
  ]
}
````

---

### 🔹 Step 4: Create a CloudFront Distribution

1. Go to the [CloudFront console](https://console.aws.amazon.com/cloudfront).
2. Click **Create Distribution**.
3. Set Origin domain as your S3 bucket (auto-fill).
4. For **Origin access**, select **Public bucket** (simple setup).
5. Default cache behavior:

   * **Viewer protocol policy**: Redirect HTTP to HTTPS
6. Leave other defaults. Click **Create distribution**.
7. Edit General settings and set "Default root object" to "index.html".  

> 💡 Wait \~10 minutes for CloudFront to deploy.

---

### 🔹 Step 5: Access Your Website

1. Copy the **Distribution domain name**, e.g.:

   ```
   https://d123abcd1234.cloudfront.net/index.html
   ```
2. Open it in your browser — you should see your static website served by CloudFront!
