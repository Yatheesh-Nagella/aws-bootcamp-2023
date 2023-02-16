# Week 0 - Billing and Architecture
### Required H/W Tasks

<ol>
 <li> Destroy your root account credentials, Set MFA, IAM role</li>
 <li> Install AWS CLI and verify it works </li>
 <li> Create a budget </li>
 <li> Create a billing alarm </li>
 <li>Create an architectural diagram (to the best of your ability) the CI/CD logical pipeline in Lucid Charts</li>
 
</ol>

---

#### Installed AWS CLI in gitpod using following code:

```
curl -fSsl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip -qq awscliv2.zip
sudo ./aws/install --update
rm awscliv2.zip
```

In the above script, we download the AWS CLI zip, unzip it & execute that to install AWS CLI, following which we remove the zip. To automatically get credentials for Amazon ECR, we would need to install ECR-Credential Helper.

Typed ```aws sts get-caller-identity``` got an error :( <br>
```aws configure ``` to configure credentials and to look up the credentials ``` aws configure list ```

![Proof of AWS CLI](assets/week0%20gitpod%20aws%20cli%20login.png)

#### AWS Budgets
![AWS Budgets](assets/week0%20budgets.png)

#### Logical Cruddur architecture
![Architecture](assets/week0%20lucid%20architecture.png)
<br>
[Lucid chart share link](https://lucid.app/lucidchart/79269e18-aa88-4c4d-aa10-fb8dcd7f43a2/edit?viewport_loc=-186%2C211%2C2349%2C905%2C0_0&invitationId=inv_d31687f9-6dfa-4e34-8d71-8b1f24c987c7) 
