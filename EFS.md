Go to any datacenter and create security group first







Create inbound rule very important :







Now go to search console - >Search for EFS




First click file system it create with default values but here need to go for customize  












Choose the security group that we created for EFS in the beginning because we have enabled required ports on it.



Click next


We are leaving it as default




Click next



















Now create EC2 instance





	
	
	
	Click edit network settings and change the subnet - for eg: 2a


	
	
	
	
	
	Create another instance and change name & subnet alone - for 2b
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	GO TO SERVER AND RUN BELOW COMMANDS:
	
	sudo -i
	apt update
	apt install nginx nfs-common -y
	ls /var/www/html/
	

	
	
	
	
	GO TO EFS -- > SELECT EFS -- > ATTACH -- > COPY PASTE COMMANDS ON BOTH SERVERS
	
	
	
	
	
	
	
	
	Copy this path to the server and mount it
	
	
	
	
	
	
	
	Now is efs is mounted
	
	
	
	
	The same step needs to be updated on second server as well
	
	Create a file at this location on server 1 & verify the on the second server same file should be there.
	
	cd /var/www/html/
	touch file1
	
	
	
	
	
	
	
	##################################################
	
	
	What is Amazon Elastic File System (EFS)?
	Amazon Web Services Elastic File System (EFS) is a fully managed, scalable file storage system that you can mount on multiple EC2 instances at the same time.
	🔹 Simple Meaning
	Think of it like a shared folder in the cloud that multiple servers can access simultaneously.
	
	🔹 Why do we use EFS?
		• 📁 Shared storage for multiple EC2 instances 
		• 📈 Automatically scales storage (no capacity planning) 
		• ⚡ High availability across multiple Availability Zones 
		• 🔄 Works like Linux file system (NFS) 
		• 🧠 Great for web servers, CMS, big data, shared logs 
	
	
	🔹 Real Example Use Case
	Imagine:
		• 3 EC2 web servers running Nginx 
		• All need same files (images, uploads) 
	👉 Instead of copying files to each server
	👉 You mount one EFS storage to all servers
	
	🔹 Steps to Create EFS (Console Method)
	
	🔸 Step 1: Open EFS Console
	Go to:
		• AWS Console → Search EFS 
	
	🔸 Step 2: Create File System
	Click:
		• Create file system 
	
	🔸 Step 3: Configure File System
		• Name: my-efs 
		• VPC: Select your VPC 
	Click Customize
	
	🔸 Step 4: Network Settings (Important)
	For each Availability Zone:
		• Select subnets 
		• Create mount targets 
		• Attach security group 
	👉 Security Group must allow:
		• NFS port 2049 
	
	🔸 Step 5: Security Group Rules
	Inbound rule:
		• Type: NFS 
		• Port: 2049 
		• Source: EC2 Security Group 
	
	🔸 Step 6: Create File System
	Click:
		• Create 
	
	🔹 Step 7: Mount EFS on EC2
	Install NFS client (Linux):
	
	sudo apt update -y
	sudo apt install nfs-common -y
	
	Create mount directory:
	
	sudo mkdir /mnt/efs
	
	Mount EFS:
	
	sudo mount -t nfs4 -o nfsvers=4.1 fs-xxxx.efs.ap-south-1.amazonaws.com:/ /mnt/efs
	
	Verify:
	
	df -h
	
	🔹 Step 8: Auto Mount (Optional but recommended)
	Edit fstab:
	
	sudo nano /etc/fstab
	Add line:
	
	fs-xxxx.efs.ap-south-1.amazonaws.com:/ /mnt/efs nfs4 defaults,_netdev 0 0
	
	🔹 EFS vs EBS (Important Interview Point)
	Feature	EFS	EBS
	Access	Multiple EC2	Single EC2
	Type	File system	Block storage
	Scalability	Auto scaling	Fixed size
	Use case	Shared storage	OS / database
	
	###############################################################
	
	U can mount the EFS via GUI using below method also
	
	
	
	
	
	
	Yes 👍 — you can fully create Amazon EFS (Elastic File System) using AWS CloudShell (CLI script).
	Below is a ready-to-run script that will:
		• Create EFS file system 
		• Create mount targets in subnets 
		• Create security group rule for NFS (2049) 
		• Output mount DNS name 
	
	🔹 EFS Creation Script (CloudShell)
	⚠️ Before running, update:
		• VPC ID 
		• Subnet IDs (at least 2 recommended) 
		• EC2 Security Group ID (where EC2 is running) 
	
	
	# -------- VARIABLES --------
	REGION="us-east-1"
	VPC_ID="vpc-xxxxxxxx"
	SUBNET_1="subnet-xxxxxxxx"
	SUBNET_2="subnet-yyyyyyyy"
	EC2_SG_ID="sg-xxxxxxxx"
	
	EFS_NAME="my-efs"
	
	# -------- CREATE SECURITY GROUP FOR EFS --------
	EFS_SG_ID=$(aws ec2 create-security-group \
	  --group-name efs-sg \
	  --description "Security group for EFS" \
	  --vpc-id $VPC_ID \
	  --query 'GroupId' \
	  --output text \
	  --region $REGION)
	
	echo "EFS Security Group: $EFS_SG_ID"
	
	# Allow NFS (2049) from EC2 SG
	aws ec2 authorize-security-group-ingress \
	  --group-id $EFS_SG_ID \
	  --protocol tcp \
	  --port 2049 \
	  --source-group $EC2_SG_ID \
	  --region $REGION
	
	# -------- CREATE EFS FILE SYSTEM --------
	EFS_ID=$(aws efs create-file-system \
	  --creation-token $EFS_NAME \
	  --performance-mode generalPurpose \
	  --throughput-mode bursting \
	  --encrypted \
	  --query 'FileSystemId' \
	  --output text \
	  --region $REGION)
	
	echo "EFS ID: $EFS_ID"
	
	# -------- CREATE MOUNT TARGETS --------
	aws efs create-mount-target \
	  --file-system-id $EFS_ID \
	  --subnet-id $SUBNET_1 \
	  --security-groups $EFS_SG_ID \
	  --region $REGION
	
	aws efs create-mount-target \
	  --file-system-id $EFS_ID \
	  --subnet-id $SUBNET_2 \
	  --security-groups $EFS_SG_ID \
	  --region $REGION
	
	# -------- GET EFS DNS NAME --------
	EFS_DNS="$EFS_ID.efs.$REGION.amazonaws.com"
	
	echo "----------------------------------------"
	echo "EFS CREATED SUCCESSFULLY"
	echo "EFS ID: $EFS_ID"
	echo "Mount DNS: $EFS_DNS"
	echo "Mount command:"
	echo "sudo mount -t nfs4 -o nfsvers=4.1 $EFS_DNS:/ /mnt/efs"
	
	🔹 After Script (EC2 Mount Steps)
	Run on EC2:
	
	sudo apt update -y
	sudo apt install nfs-common -y
	sudo mkdir /mnt/efs
	sudo mount -t nfs4 -o nfsvers=4.1 fs-xxxx.efs.us-east-1.amazonaws.com:/ /mnt/efs
	
	🔹 What this script does
	✔ Creates EFS
	✔ Creates security group for NFS (2049)
	✔ Allows EC2 to connect
	✔ Creates mount targets in subnets
	✔ Outputs mount DNS
	
	🔹 Important Notes
		• EFS is regional (not single AZ) 
		• Must use at least 2 subnets for HA 
		• Security group is critical (port 2049) 
		• EC2 and EFS must be in same VPC
	
	######################################################3
<img width="1168" height="14787" alt="image" src="https://github.com/user-attachments/assets/f64d37b9-2d77-45ce-b865-9bf4bbb5482c" />
