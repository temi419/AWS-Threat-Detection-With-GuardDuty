<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Threat Detection with GuardDuty

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-guardduty)

**Author:** David Adeleye  
**Email:** davidadley58@gmail.com

---

![Image](http://learn.nextwork.org/eager_green_serene_nutmeg/uploads/aws-security-guardduty_sm42x3y4)

---

## Introducing Today's Project!

### Tools and concepts

The services we used were GuardDuty, CloudFormation, S3, and CloudShell. Key concepts we learnt include SQL + command Injection,using linux commands like wget,cat, jq, and malware protection.

### Project reflection

This project took me approximately 4 hours. The most challenging part was the CloudShell gettin access to the AWS environment, as the URL I was using was wrong as well as my S3 bucket was wrong. It was most rewarding to understand CloudShell and S3 from a attackers perspectives and reveal the victims protected file.

I did this project today to learn about threat detection and GuardDuty + techniques like SQL/command injection. This project definetly tested my problem solvoing skills.

---

## Project Setup

To set up for this project, we deployed a CloudFormation template that launches a insecure web app(OWASP Juice shop). The three main components are the web app infastructure, an S3 bucket and GuardDuty protecting our environment .

The web app deployed is called OWASP Juice Shop. TO practice our GuardDuty skills, we will attack the Juice Shop and then visit the GuardDuty console to detect and analyze its finding- does it pick up on our attacks to our web app? 

GuardDuty is an AI-powered threat detection service, which means it is designed to help us find security attacks or vulnerablities that affectrs our AWS resources/environment.Once it detects something unusual, it's up to us to investigate.

![Image](http://learn.nextwork.org/eager_green_serene_nutmeg/uploads/aws-security-guardduty_n1o2p3q4)

---

## SQL Injection

the first attack we performed on the web app is SQL injection, which means injecting malicious SQL code that manipulates a result from our webb app. SQL injection is a security risk because it can let attackers bypass logins, delete data or delete/edit data.

My SQL injection attck involved entering the code ' or 1=1;-- into the email field of the web app's login page.This means the login query will always evaluate to true(i.e our database is manipulated into telling our web app this login exists).

![Image](http://learn.nextwork.org/eager_green_serene_nutmeg/uploads/aws-security-guardduty_h1i2j3k4)

---

## Command Injection

Next, I used command injection, which manipulates the web app's web server to run code that has been entered e.g. in a form. The Juice Shop web app is vulnerable to this because it does not sanitize user inputs i.e. does not block scripts.

To run command injection, I entered it into username section after getting to the login section The script will be entered into username section to be read as a username, but in reality it puts the system into a illusion, instead of reading a name it is reading and running script.



![Image](http://learn.nextwork.org/eager_green_serene_nutmeg/uploads/aws-security-guardduty_t3u4v5w6)

---

## Attack Verification

To verify the attack's success, we visited the public exposed credential files  (i.e. credentials.json). This page showed us access keys that can give anyone access to the resources that represents our EC2 instance's access to our AWS environment. We can use the keys to get the same level of access. 

![Image](http://learn.nextwork.org/eager_green_serene_nutmeg/uploads/aws-security-guardduty_x7y8z9a0)

---

## Using CloudShell for Advanced Attacks

The attack continues in CloudShell, because this is a tool we can use to run commands that uses the credentials we've stolen. CloudShell will be our medium for doing suspicious things like stealing data from an S3 bucket.

In CloudShell, we used wget to download the exposed credentials file into our CloudShell environment. Next, we ran a command using cat and jq to read the download file and format it nicely so the credentials (in JSON) is easy to understand.

We then set up a profile, called stolen,to store and save all of the stolen credentials. We had to create a new profile because the hacker doesn't inherently have access to the victim's AWS environment. We'll need to use the profile to switch permission settings. 

![Image](http://learn.nextwork.org/eager_green_serene_nutmeg/uploads/aws-security-guardduty_j9k0l1m2)

---

## GuardDuty's Findings

After performing the attack, GuardDuty reported a finding within 15 minutes. Findings are notifications from GuardDuty that something suspicious has happened, and they give you additional details about the who/what/when of the attack.

GuardDuty's finding was called UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS , which means credentials belonmging tio an EC2 instance in my environment were being use in another account Anomaly detection was used because this was unusual behaviour.



GuardDuty's detailed finding reported that an S3 bucket was affected, the action; that was done using the stolen credentials was GetObject; and the EC2 instance whose credentials were leaked. The IP address + location of the actor was also available. 

![Image](http://learn.nextwork.org/eager_green_serene_nutmeg/uploads/aws-security-guardduty_v1w2x3y4)

---

## Extra: Malware Protection

To test Malware protection, I uploaded some Malware (EICAR file)to test out if GuardDuty would react. The uploaded file won't actually cause damage because the test file is only designed to alert antivirus software.

Once we uploaded the malware, GuardDuty instantly triggered a finding called Object:S3/MaliciousFile. This verified that GuardDuty could successfully detect malware. It was also mentioned that the threat type is a EICAR Test-File(which means its not a virus).

![Image](http://learn.nextwork.org/eager_green_serene_nutmeg/uploads/aws-security-guardduty_sm42x3y4)

---
