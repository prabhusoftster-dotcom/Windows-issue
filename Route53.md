

1.) Create 2 ubuntu nginx server and make sure the website is browsing

2.) Domain purchase from big rock











Paste ur purchased domain name and choose public for web server



Name server will tell you the location of the domain










Copy and paste the first 2 name server record in domain portal



Updated 2 name server in domain portal




Now create record








Give the ip address of our server ( EC2 public ip )












It took half an hour to work at last ( try using http://globalacademy.buzz )





-> create LB & target group


Covert IPV4 to Load balancer DNS









Status need to change to in-sync





-------------------------------------------
Troubleshooting steps:

nslookup globalacademy.buzz   - inside server  & outside of the server ( laptop )

On laptop it took 1/2 hr time to resolve the domain name ( externally )



systemctl status nginx

If not running:
systemctl start nginx
systemctl enable nginx

Verify your EC2 security group

Ensure inbound rules include:

HTTP   80    0.0.0.0/0
HTTPS  443   0.0.0.0/0

------------------------------------------------------------------



Routing policy:





####################################################################

Route 53 is used to:
	• Map domain names to AWS resources 
	• Control traffic routing 
	• Ensure high availability 
	• Monitor application health

Amazon Route 53 is a highly available and scalable DNS (Domain Name System) web service in Amazon Web Services.
In simple terms 👉 it connects your domain name (like example.com) to your actual server (IP / AWS resource).

🔹 Why Route 53 is Used
1. 🌐 Domain Name Resolution (DNS)
	• Converts:

www.mywebsite.com → 192.168.1.1 (IP address)
	• So users don’t need to remember IP addresses 

2. 🧭 Traffic Routing
Route 53 can control how traffic is routed:
	• Simple routing → one server 
	• Weighted routing → split traffic (e.g., 70% / 30%) 
	• Latency-based → nearest region 
	• Failover → backup server if primary fails 

3. 🚀 High Availability (Failover)
	• Automatically redirects traffic if your server is down 
	• Used with: 
		○ ELB (Load Balancer) 
		○ EC2 
		○ S3 static websites 
4. 🌍 Domain Registration
	• You can buy/manage domains directly
(like GoDaddy alternative) 

5. 🔐 Health Checks & Monitoring
	• Route 53 checks if your application is: 
		○ Up ✅ 
		○ Down ❌ 
	• Automatically reroutes traffic if unhealthy 

🔹 Real-Time Example
Let’s say you created a website on EC2:
	• Server IP: 54.210.10.20 
	• Domain: myapp.com 
👉 Using Route 53:
	• Create Hosted Zone 
	• Add A record 
	• Map:

myapp.com → 54.210.10.20
Now users can access:

http://myapp.com
instead of IP

🔹 Common AWS Services Used with Route 53
	• EC2 (Web servers) 
	• S3 (Static websites) 
	• CloudFront (CDN) 
	• ELB (Load Balancer) 

🔹 Key Components
	• Hosted Zone → Container for DNS records 
	• Record Sets → A, CNAME, MX, etc. 
	• Name Servers (NS) → Provided by AWS 
	• TTL → Cache time 
	
#########################################
<img width="1297" height="11352" alt="image" src="https://github.com/user-attachments/assets/9c429e76-d4bf-4afb-8f5a-e3a3e3b6fbf3" />
