# Week 0 - Billing and Architecture
### Required H/W Tasks

<ol>
 <li> Install AWS CLI and verify it works </li>
 <li>Destroy your root account credentials, Set MFA, IAM role</li>
 <li>Create an architectural diagram (to the best of your ability) the CI/CD logical pipeline in Lucid Charts</li>
 <li>Research the technical and service limits of specific services and how they could impact the technical path for technical flexibility.</li>
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
