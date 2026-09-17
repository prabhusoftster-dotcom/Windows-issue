






1.) Create VPC 
2.) Create subnet
3.) Create Internet gateway
4.) Create route table
5.) Associate the web-server to VPC 
6.) Create a webserver with ( associate VPC + Subnet already created ) choose auto-enable public ip ( Create new SG  )
7.) Edit Route table -> IGW
8.) Attach IGW ->  VPC 









AWS has default Network of Class - B



STEP-1: CREATE VPC
VPC -- > CREATE VPC -- > NAME: SWIGGY -- > CIDR: 10.0.0.0/16 -- > CREATE

SUBNET = ROOM

STEP-2: CREATE SUBNET [PUBLIC = HALL]
SUBNET -- > CREATE -- > VPC: SWIGGY -- > NAME: WEB-SUBNET  -- > CIDR: 10.0.0.0/24 -- > CREATE

STEP-3: CREATE A INTERNET GATEWAY [IGW = MAIN GATE]
INTERNET GATEWAY -- > CREATE -- > NAME: SWIGGY-IGW -- > CREATE -- > ATTACH TO VPC -- > SWIGGY

STEP-4: CREATE ROUTE TABLE
ROUTE TABLE -- > NAME: SWIGGY-WEB-RTB -- > VPC: SWIGGY -- > CREATE
EDI ROUTES -- > ADD -- > 0.0.0.0/0 -- > IGW : SWIGGY -- > ADD

ASSOCIATE THE WEB SERVER TO THE ROUTE TABLE.
SUBNET ASSOCIATION -- > SELECT WEBSUBNET -- > ASSOCIATE.

VPC Endpoints: provide a secure and private connection between your VPC & AWS services, without requiring access over the public internet.
This is useful for scenarios where you want to avoid exposing sensitive data to the internet while enabling private communication between resources in your VPC and services like Amazon S3, DynamoDB, or other AWS services.

VPC dashboard

















Create subnet


















Create internet gateway



Attach the gateway to VPC   - IGW to VPC








Create route table










Connecting the route table to Internet gateway (IGW)   RT - IGW




Edit Subnet because 








Create a new web server 

Edit network settings - select the VPC that we created here ( The webs server is attached to VPC )















Actual Linking Flow (Very Important)
Here’s the exact chain in AWS:

VPC 
 ├── Subnet 
 │     └── EC2 (Web Server)
 │
 ├── Route Table 
 │     └── Associated to Subnet
 │
 └── Internet Gateway (IGW)
       └── Attached to VPC

🧩 Step-by-Step Linking Explained
1️⃣ VPC → Subnet
	• Your subnet is created inside the VPC 
	• Example:

VPC: 10.0.0.0/16
Subnet: 10.0.1.0/24
👉 This is already linked when you create the subnet

2️⃣ Subnet → Route Table (CRITICAL)
This is where many setups fail.
👉 You must associate the route table with your subnet
✔ Check:
	• Go to Route Table → Subnet Associations 
	• Ensure your subnet is listed 

3️⃣ Route Table → Internet Gateway
Your route table must have this route:

Destination: 0.0.0.0/0
Target: Internet Gateway (igw-xxxx)
👉 This tells AWS:
	“Send all internet traffic to IGW”

4️⃣ Internet Gateway → VPC
	• IGW must be attached to the VPC 
✔ Check:
	• VPC → Internet Gateways → Attached VPC 

5️⃣ EC2 (Web Server) inside Subnet
	• Your EC2 instance must be launched in that subnet 
	• And must have: 
		○ Public IP (auto-assign ON) 

🔄 Full Traffic Flow (Real Scenario)               
🚀 When user opens your website:
	1. User hits your EC2 Public IP 
	2. Request enters via Internet Gateway 
	3. IGW → forwards to VPC 
	4. Route table → directs traffic to subnet 
	5. Subnet → delivers to EC2 web server 

	Subnet is created inside the VPC
📤 When server responds:
	1. EC2 sends response 
	2. Route table checks:

0.0.0.0/0 → IGW
	3. Goes to IGW 
	4. IGW → sends back to internet 

🔥 Final Checklist (Quick Debug)
Make sure ALL are true:
✔ Subnet is in correct VPC
✔ Route table is associated with subnet
✔ Route table has:

0.0.0.0/0 → IGW
✔ IGW is attached to VPC
✔ EC2 has Public IP enabled
✔ Security Group allows:
	• Port 80 (HTTP) 
	• Port 22 (SSH)


#########################

“Although the Internet Gateway is attached to the VPC, traffic will only flow through it if the route table explicitly directs traffic (0.0.0.0/0) to the IGW. The route table controls which subnets can access the internet.”

The Internet Gateway is attached to the VPC, but the Route Table decides which traffic actually uses it.

###########################
<img width="1507" height="11464" alt="image" src="https://github.com/user-attachments/assets/8244f213-40b8-412f-ad31-76770c5481bf" />
