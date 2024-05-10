
# Create Users in EC2 Instances Automattically through AWS SECRETS Manager

### Document Explain
```
Document.txt - This is the document which is used in SSM/AWS Systems Manger to create automation. This document incluse 2 scripts which will first delete users which are not in current secrets manager. Then It will create new users if there is any new user entry in aws secrets manager. Users should be create as name and then ssh-public_key. It will create the users on ec2 and add public key in authorized_keys file.    
NOTE: Ec2 Instance on which you need to enable this automation should have secrets manager and ssm full access 
```

### Step 1 - Create Document in AWS SSM

1. Go to systems manager in AWS and create a Document in `owned by me` and select `automation` then & paste the Document code.

### Step 2 - Create IAM Role for Eventbridge 

1. Create a IAM role - Click on Create > select AWS ACCOUNT and this aws account.
2. For the polices select below policies 
```
CloudWatchFullAccess 
CloudWatchFullAccessv2
AmazonSSMAutomationRole
```
 
3. Then create role
4. Now Select the role again and go to `Trust relationships` and edit `trust policy` and paste below content
```

```

### Step 3 - Create EVENTBRIDGE Rule 
1. Go to AWS Event bridge and create a new rule. Select
2. Give rule any name and select `Rule with an event pattern`
3. For Event Source select `others`
4. For Creation Method select `Custom pattern (JSON editor)`
5. For Event Pattern paste below content and replace ARN of secret manager
```
{
  "source": ["aws.secretsmanager"],
  "detail": {
    "eventSource": ["secretsmanager.amazonaws.com"],
    "eventName": ["UpdateSecret", "PutSecretValue"],
    "requestParameters": {
      "secretId": ["arn:aws:secretsmanager:{REGION}:{ACCOUNT-ID}:secret:{SECRET-NAME}"]
    }
  }
}
```
6. Select `System Manager Automation` as Target and Document which we created in step 1
7. For `Execution role` select existing role and select role created in step 2
8. Click Next asn review all configuration 


### Step 4 - Test Automation by adding user in Secrets Manager

1.  Create a user in Seccret Manger which you used in Document script to fetch details.
2.  After adding the user a Command will run in `Run Command`in ssm and user will gets created.

          





 
      
