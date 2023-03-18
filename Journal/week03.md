# Week 3 — Decentralized Authentication

## Required Homework

### 1. Provision via ClickOps a Amazon Cognito User Pool

**Amazon Cognito**
AWS Cognito is a powerful and flexible identity and access management service that enables developers to add user authentication, authorization, and management features to their applications quickly and easily

**User Pools**
Amazon Cognito user pools are a managed service that lets you add secure authentication and authorization to your apps, and can scale to support millions of users.

#### Provision Cognito User Pool
Here is the user pool I created using the AWS console
![AWS Cognito user Pool]()

### 2. Install and configure Amplify client-side library for Amazon Congito

#### Install AWS Amplify
```sh
cd frontend-react-js
npm i aws-amplify --save
```


#### Configure Amplify
I configured amplify by importing it into my `App.js` and add the configure script below

```js
import { Amplify } from 'aws-amplify';

Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_APP_AWS_PROJECT_REGION,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_AWS_USER_POOLS_CLIENT_ID,
  "oauth": {},
  Auth: {
    // We are not using an Identity Pool
    // identityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID, // REQUIRED - Amazon Cognito Identity Pool ID
    region: process.env.REACT_APP_AWS_PROJECT_REGION,           // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,         // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_AWS_USER_POOLS_CLIENT_ID,   // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
  }
});

```


### 3. Implement API calls to Amazon Coginto for custom login, signup, recovery and forgot password page

In `SigninPage.js` I imported `Auth` from `aws-amplify` and updated the `onsubmit()` function

```js
import { Auth } from 'aws-amplify';


const onsubmit = async (event) => {
    setErrors('')
    event.preventDefault();
      Auth.signIn(email, password)
        .then(user => {
          localStorage.setItem("access_token", user.signInUserSession.accessToken.jwtToken)
          window.location.href = "/"
        })
        .catch(error => {  
          if (error.code == 'UserNotConfirmedException') {
            window.location.href = "/confirm"
          }
        setErrors(error.message)
      });
    return false
  }
  
```
Here is signin page responding with the right error when user email or password is not found or incorrect

To try sigin, I maually created a used from the AWS console and I changed the user's password and updated its confirm status using:

```sh
aws cognito-idp admin-set-user-password \
  --user-pool-id <your-user-pool-id> \
  --username <username> \
  --password <password> \
  --permanent
```


Fixed CORS errors by updating the cors configuration in `app.js` in `backend-flask` with
```py
cors = CORS(
  app, 
  resources={r"/api/*": {"origins": origins}},
  headers=['Content-Type', 'Authorization'], 
  expose_headers='Authorization',
  methods="OPTIONS,GET,HEAD,POST"
)
```

