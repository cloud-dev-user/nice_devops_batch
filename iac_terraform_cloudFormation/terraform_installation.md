````markdown
# 🛠️ Terraform Installation & Setup Guide

This guide helps you install and configure [Terraform](https://developer.hashicorp.com/terraform) on **Linux**.

---

## 📦 Install Terraform

### 🔹 Linux (64-bit)

```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt-get update
sudo apt-get install terraform
````

> ✅ To verify:

```bash
terraform -version
```
