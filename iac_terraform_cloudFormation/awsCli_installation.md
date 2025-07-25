````markdown
# 🚀 AWS CLI v2 Installation & Configuration Guide

This guide helps you install and configure the AWS Command Line Interface (CLI) v2 on **Linux**, **macOS**, and **Windows**.

---

## 🛠️ Prerequisites

- Admin/root access
- AWS IAM credentials (Access Key ID & Secret Access Key)

---

## 📦 Step 1: Install AWS CLI v2

### 🔹 Linux (64-bit x86)

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
````

### 🔹 macOS (Intel / M1 / M2)

```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

### 🔹 Windows

1. Download the MSI installer: [AWSCLIV2.msi](https://awscli.amazonaws.com/AWSCLIV2.msi)
2. Run the installer and follow on-screen instructions.

---

## ✅ Step 2: Verify Installation

```bash
aws --version
```

You should see output like:

```bash
aws-cli/2.x.x Python/3.x.x <OS> exe/x86_64 prompt/off
```

---

## ⚙️ Step 3: Configure AWS CLI

Run:

```bash
aws configure
```

Provide the following when prompted:

```text
AWS Access Key ID [None]: <your-access-key>
AWS Secret Access Key [None]: <your-secret-key>
Default region name [None]: us-east-1
Default output format [None]: json
```

---

## 🔍 Step 4: Test the Setup

```bash
aws sts get-caller-identity
```

Expected Output:

```json
{
  "UserId": "XXXXXXXXXXXXXXXXXXX",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/your-username"
}
```



