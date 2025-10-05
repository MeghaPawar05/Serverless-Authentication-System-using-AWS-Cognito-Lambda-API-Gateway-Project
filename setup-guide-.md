# 🔵 Setup Guide – Serverless API with Cognito Authentication


## **1. Create a Lambda Function**

1. Go to **AWS Management Console → Lambda → Create function**.
2. Select **Author from scratch**.

   * Function name: `Lambda-Function'
`.
   * Runtime: Python/Node.js/Java (your choice).
3. Add your code. Example (Python):

```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Hello, your request was successful!"
    }
```

4. Deploy the function.
5. <img width="1893" height="793" alt="image" src="https://github.com/user-attachments/assets/b8a34593-7d24-4323-9683-7d1fe8c3b9cc" />


## **1. Create a Cognito User Pool**

1. Go to **AWS Management Console → Cognito → User Pools**.
2. Click **Create User Pool**.
3. Configure:

   * Pool name (e.g., `User pool - d4mfdn`).
   * Sign-in options → Choose `Email` or `Username`.
   * Enable **Self sign-up** if required.
   * Configure password policy (default is fine).
4. Create a **User Pool Client** (e.g., `MyAppClient`):

   
5. Save the **User Pool ID** and **App Client ID** → you’ll need them later.
6. <img width="1913" height="843" alt="image" src="https://github.com/user-attachments/assets/40332d54-853d-452a-825f-912ff3e82947" />


---

## **3. Create an API Gateway**

1. Go to **Amazon API Gateway → Create API → REST API** (or HTTP API for simpler setup).
2. Create a **resource** (e.g., `/hello`).
3. Create a **method** → `GET`.
4. Choose **Lambda Function** as integration → Select your Lambda.
5. Deploy the API → Create a stage (e.g., `dev`).
6. Copy the **Invoke URL** (e.g., `https://abc123.execute-api.ap-south-1.amazonaws.com/dev/hello`).
7. <img width="1912" height="852" alt="image" src="https://github.com/user-attachments/assets/715c34c4-799a-4506-af32-d0a140928444" />


---

## **4. Add Cognito Authorizer to API Gateway**

1. In API Gateway, go to **Authorizers → Create new authorizer**.

2. Select:

   * Type: **Cognito**.
   * Name: `CognitoAuthorizer`.
   * Choose your **Cognito User Pool**.
   * Region: your pool’s region.

3. Save it.

4. Go to your API method (`/hello → GET`) →

   * Click **Method Request**.
   * Under **Authorization**, select `CognitoAuthorizer`.
   * Deploy changes again.
   * <img width="1912" height="845" alt="image" src="https://github.com/user-attachments/assets/1c33d264-845b-40f6-9ca0-fcecac4b2608" />



## **5. Test the Setup**

### Step A: Get a Token

* Use AWS Cognito Hosted UI OR AWS CLI to sign in and get a **JWT token**.
  Example with AWS CLI:
  <img width="1919" height="585" alt="Screenshot 2025-10-05 132033" src="https://github.com/user-attachments/assets/8848a1d6-cb6a-4bd3-8333-4653355ed002" />





✅ If token is valid → You’ll get response from Lambda.
❌ If token is invalid → API Gateway denies access.


## **Final Workflow**

1. User signs up/signs in → gets JWT token from **Cognito**.
2. User sends request to **API Gateway** with token.
3. **API Gateway Authorizer** validates token with Cognito.
4. If valid → request goes to **Lambda**.
5. Lambda executes code (with/without DB) and returns response.


