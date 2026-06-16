# AWS Auto-Scaling Web App

Hi! I built this mini-project to get some hands-on experience with AWS infrastructure. I wanted to learn how to handle traffic spikes without servers crashing, and how to automatically save money when traffic is low. 

### How I built the architecture
To make sense of the different AWS services, I like to think of this setup like a busy restaurant:

* **EC2 Instances (The Cashiers):** These are the actual Linux servers running my Nginx web page.
* **Application Load Balancer (The Greeter):** This sits at the front door. Instead of letting one server get overwhelmed, the ALB routes incoming traffic to whichever server has the least load.
* **Auto Scaling Group (The Manager):** I set this up to watch the CPU usage. If CPU hits 50% (like a sudden dinner rush), the ASG automatically spins up new servers. When traffic drops, it terminates the extra servers so I'm not paying for unused compute power.
* **Security Groups (The Bouncers):** I configured two security groups. The ALB accepts public internet traffic, but the EC2 instances are completely locked down—they only accept traffic directly from the ALB.

### What's inside this repo?
* **`user-data.sh`:** This is the bash script I attached to my EC2 Launch Template. Whenever the Auto Scaling Group creates a new server, this script automatically runs on boot to update the system, install Nginx, and create a custom webpage showing that specific server's Instance ID. 

### How to test it
1. Deploy the infrastructure in the AWS Console.
2. Grab the DNS link provided by the Application Load Balancer.
3. Paste it into a browser and hit refresh a few times. You will see the Instance ID change on the screen, proving that the load balancer is actively routing your connection between the different servers running in the background.
