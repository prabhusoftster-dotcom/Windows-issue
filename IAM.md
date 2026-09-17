















https://160284305395.signin.aws.amazon.com/console
Ram
Chennai@123

Login as the new user after changing the password


You will be able to create only s3 bucket on other access will work becoz of IAM permission no other access is enabled for the user




While creating the user has below permission only




No access for eg :





To give access to resource or additional access to any resource below is the procedure









---> the user now has permission to create EC2 instance

How to enable console access for user











































Before that install aws cli in the server
When 2 services want to communicate we need IAM roles




Now set no role after work is completed







How to revoke session role



















Provide the bucket that you want to restrict



Give policy name : s3-custom-policy









----------------------------------------

How to change the password settings





--------------------------------------------






--------------------------------------------------------
How to disable console if user leaves the company








--------------------------------------------------------------------

If you lose your AWS access keys (Access Key ID + Secret Access Key), the impact depends on who had them and whether anyone else now has them.
🚨 Worst-case scenario (keys exposed)
If someone else gets your keys, they can act as you via the AWS CLI or API:
	• Launch resources (EC2, Lambda, etc.) 
	• Access or delete S3 data 
	• Modify IAM permissions 
	• Run up huge bills 
This is why AWS treats access keys like passwords.

🤷 If you just lost them (not exposed)
If you simply misplaced them and no one else has them:
	• Nothing immediately breaks unless something depends on those keys (scripts, apps, CI/CD) 
	• Those services will start failing authentication 

🔧 What you should do immediately
	1. Disable or delete the lost key
Go to AWS IAM Console
→ Users → Security credentials → Access keys
→ Deactivate or delete the key 
	2. Create a new key (if needed)
Replace it in your apps/scripts 
	3. Check for suspicious activity
Review: 
		○ AWS CloudTrail logs 
		○ Billing dashboard for unexpected charges 
	4. Rotate keys regularly
Don’t reuse old ones 

🧠 Best practices (to avoid this situation)
	• Prefer IAM roles instead of long-term keys (especially for EC2, Lambda) 
	• Store secrets in: 
		○ AWS Secrets Manager 
		○ AWS Systems Manager Parameter Store 
	• Never hardcode keys in code or push them to GitHub

------------------------------------------
arn:aws:s3:::globaldataproject001
![Uploading image.png…]()
