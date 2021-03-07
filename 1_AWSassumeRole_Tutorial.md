March 6th

Focus - Critique of following tutorial 

[Assume an IAM role using the AWS CLI (amazon.com)](https://aws.amazon.com/premiumsupport/knowledge-center/iam-assume-role-cli/)



## Problem 
1. They make you create a policy with permissions and attach it to a newly created user. Although this doesn't sound like a problem, it grants the user permissions we would use **AFTER** the role has been assumed. So what does this mean? If you run into trouble like not being able to assume the role, you will use the CLI with the same permissions as the role you want to assume. 

> SOLUTION - Attach a policy to the user with **ONLY** permissions to assume the role. After that we can use AWS services by using the permissions exclusive to the assumed role.



2. A simple problem I have is that **I read things literally**. In the section **Assume the IAM** role it specifically says *"To assume the IAM role, run this command:"*  

> ```aws sts assume-role --role-arn "arn:aws:iam::123456789012:role/example-role" --role-session-name AWSCLI-Session``` 

After running this I was able to use AWS services with permissions granted *thinking I didn't have to do anything else since I had just ran the command to assume the role*. Although this was the case, I was unable to verify I had assumed the role by running the following command. 

>  ``` aws sts get-caller-identity```

Unable to verify the role but being able to use its permissions had me stumped. I had to mess around with AWS through the management console to diagnose what went wrong. I checked what policies I had attached to the newly created user and role.  Everything seemed to look right. 

In the tutorial it specifically says ```The AWS CLI command outputs several pieces of information. Inside the credentials block you need the **AccessKeyId**, **SecretAccessKey**, and **SessionToken**.``` Trying to find the solution quickly and accurately, I didn't translate this part of the tutorial into "you need to set up environmental variables to assume the role." In hindsight knowing the solution to my problem, it says that inside of the outputted information that we would **need** the AccessKeyID, SecretAccessKey and SessionToken. But what for? Going through this tutorial for the first time I had ignored the last step of the tutorial *thinking one command was all I **literally** needed to assume the role* when all I needed to do was follow the last step which is to set up environmental variables.



## Epiphany

This is still a good tutorial, I just have trouble getting too stuck on one step and trying to understand it when sometimes an extra step is all I need to piece it together and understand. Also, I believe my solution for problem#1 is *good practice*. The tutorial misses that lesson by making ONE policy attach to BOTH the user and role when there should be two policies created, one for each with different permissions.  



## Lessons

1. Abstraction of how much access permissions should be given to a user and a role. 
2. Don't read things as too literal.
3. Some things need extra configuring to work. I can always go back, fix and diagnose if problems occur.