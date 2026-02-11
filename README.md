<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Threat Detection with GuardDuty

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-guardduty)

**Author:** saqibh49@gmail.com  
**Email:** saqibh49@gmail.com

---

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-security-guardduty_v1w2x3y4)

---

## Introducing Today's Project!

### Tools and concepts

The services I used were Amazon GuardDuty, IAM, EC2, and S3. Key concepts I learnt include threat detection, credential exfiltration, anomaly detection, cross-account access monitoring, and how GuardDuty identifies suspicious activity using behavioral analysis.

### Project reflection

This project took me approximately 2 hours

 I ddid this project because I have a goal of becoming a cloud engineer and I have to learn these things if I want to reach that goal  

---

## Project Setup

To set up for this project, I deployed a CloudFormation template that launches a new VPC, subnets, security group, internet gateway, S3 bucket, GuardDuty, route tables, vpc endpoints, elastic load balancer, and launch templates. The three main components are EC2 Instance, networking resources, and a CloudFront distribution. 

The web app deployed is called OWASP Juice Shop. To practice my GuardDuty skills, I will attempt to hack the OWASP Juice Shop since it is intentionally built with data vulnerabilities for security engineers to practice on. 

GuardDuty is an AI tool that scans your AWS account for suspicious activity and alerts you right away if it detects anything. In this project, it will search for and hopefully find me when I hack into the resources in my EC2. 

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-security-guardduty_n1o2p3q4)

---

## SQL Injection

The first attack I performed on the web app is SQL injection, which means inserting malicious SQL code into an application’s input fields in order to manipulate the database behind it. SQL injection is a security risk because it can allow attackers to view, modify, or delete sensitive data, bypass authentication, and potentially gain unauthorized access to the system if input validation and proper query parameterization are not implemented.

My SQL injection attack involved entering ' or 1=1;-- in the email field and 1 in the password field. This means I manipulated the SQL query so that the condition 1=1 always evaluates to true, and the -- commented out the rest of the query, allowing me to bypass authentication and log in without valid credentials.

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-security-guardduty_h1i2j3k4)

---

## Command Injection

Next, I used command injection, which is an attack where malicious system commands are inserted into a vulnerable application input field and executed on the underlying server. The Juice Shop web app is vulnerable to this because it does not properly sanitize or validate user input before passing it to the system shell, allowing injected commands to run with the application's permissions.

To run command injection, I entered teh javascript command into the username field. The script will download the data from the EC2 instance and enter it into a credentials.json file so anyone who accesses the web app can see the leaked data. 

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-security-guardduty_t3u4v5w6)

---

## Attack Verification

To verify the attack's success, I went to the address where the credentials would be saved based on the command I ran. The credentials page showed me the key pair, token, and expiration date of the token. 

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-security-guardduty_x7y8z9a0)

---

## Using CloudShell for Advanced Attacks

The attack continues in CloudShell, because AWS assigns each CloudShell session a different user, so I can attempt to use the stolen keys to login as if I'm a different person. GuardDuty will see that the keys im trying to use are not associated with my user and go off. 

In CloudShell, I used wget to retrieve the file from AWS and pull it into CloudShell. Next, I ran a command using cat and jq to show me the contents of the file and interpret the json into plain text so its easy to read. 

 then set up a profile, called stolen to attempt to log into my AWS with the stolen credentials. I had to create a new profile because the default profile created in CloudShell has the same permissions as the user thats logged into the console, so I need a new profile that doesn't have authorization to access my aws data in order to hack into it. 

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-security-guardduty_j9k0l1m2)

---

## GuardDuty's Findings

After performing the attack, GuardDuty reported a finding within about 30 seconds. Findings are essentially reports of activity that GuardDuty finds to be problematic. 

GuardDuty's finding was called UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS, which means credentials created for an EC2 instance role were used from a different AWS account, indicating possible credential suspicious activity. Anomaly detection was used because GuardDuty monitors normal credential usage patterns and flagged this activity as suspicious when the instance role credentials were accessed from an unexpected external account.

GuardDuty's detailed finding reported that credentials created exclusively for an EC2 instance role were used from a remote AWS account, indicating potential credential exfiltration and unauthorized access.

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-security-guardduty_v1w2x3y4)

---

## Extra: Malware Protection

To test Malware Protection, I uploaded a malware file to my S3 bucket. The uploaded file won't actually cause damage because it is purpose built to test anti-malware software on computers. 

Start your answer with 'Once I uploaded the file, GuardDuty instantly triggered an alert showing that a piece of malware had been added to my S3 bucket. This verified that the malware protection from GuardDuty was working. 

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-security-guardduty_sm42x3y4)

---
