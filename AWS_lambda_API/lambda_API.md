````markdown
# 🚀 AWS Lambda + API Gateway Hands-on Guide

## 🎯 Objective

Expose a **Lambda function** via **API Gateway** as a public HTTP endpoint.

---

## 🧰 Prerequisites

| Item             | Description                         |
|------------------|-------------------------------------|
| AWS Account       | Required                           |
| IAM Permissions   | Lambda + API Gateway + CloudWatch  |
| Region            | Recommended: `us-east-1`           |

---

## 🔧 Step-by-Step Setup

---

### 🔹 Step 1: Create a Lambda Function

1. Go to [AWS Lambda Console](https://console.aws.amazon.com/lambda/)
2. Click **Create function**
   - Choose **Author from scratch**
   - Function name: `HelloApiLambda`
   - Runtime: `Python 3.12` or `Node.js 20.x`
3. Click **Create function**

#### 🔸 Example Code

**Python:**
```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Hello from Lambda via API Gateway!"
    }
````

**Node.js:**

```javascript
exports.handler = async (event) => {
    return {
        statusCode: 200,
        body: "Hello from Lambda via API Gateway!",
    };
};
```

> After pasting the code, click **Deploy**.

---

### 🔹 Step 2: Create an API Gateway (HTTP API)

1. Go to [Amazon API Gateway Console](https://console.aws.amazon.com/apigateway)
2. Click **Create API**
3. Choose **HTTP API**
4. Click **Build**

---

### 🔹 Step 3: Connect Lambda to API Gateway

1. In **Configure routes**:

   * Resource path: `/hello`
   * Method: `GET`
2. Integration:

   * Integration type: **Lambda function**
   * Select your function: `HelloApiLambda`
   * Click **Add integration**
3. Click **Next**
4. Leave default stage name (`$default`)
5. Click **Create**

---

### 🔹 Step 4: Test the API

1. Copy the **API endpoint URL** (shown after creation), e.g.:

   ```
   https://a1b2c3d4.execute-api.us-east-1.amazonaws.com/hello
   ```
2. Open in browser or use curl/Postman:

   ```
   curl https://a1b2c3d4.execute-api.us-east-1.amazonaws.com/hello
   ```

✅ You should see:

```
Hello from Lambda via API Gateway!
```

---

## 🧹 Cleanup

To avoid charges:

* **Delete API Gateway API**
* **Delete the Lambda function**

---
