# Scalable Web App (AWS ALB + Auto Scaling) 

Welcome to my AWS mini-project! This project demonstrates how to build a highly available and scalable web architecture using Amazon Web Services. 

## How It Works (The Fast-Food Analogy)
To make this easy to understand, imagine this architecture as a super-popular fast-food restaurant:

* **The Cashiers (EC2 Instances):** These are the actual servers running the website. 
* **The Greeter (Application Load Balancer):** Stands at the front door and directs customer traffic to the shortest line so no single cashier gets overwhelmed.
* **The Store Manager (Auto Scaling Group):** Watches how busy the cashiers are. If a sudden rush of customers arrives (high CPU usage), the manager instantly creates new cashiers. When the rush is over, the manager sends the extra cashiers home to save money.
* **The Bouncers (Security Groups):** Keeps the servers safe. The Front Door Bouncer allows public internet traffic to the Greeter, but the Kitchen Bouncer ensures nobody can talk to the Cashiers directly unless they go through the Greeter first!

## Architecture Components
* **Compute:** Amazon EC2 (Amazon Linux 2023, t2.micro)
* **Traffic Distribution:** Application Load Balancer (ALB)
* **Automation:** Auto Scaling Group (Min: 1, Max: 4, Target CPU: 50%)
* **Security:** AWS Security Groups
* **Web Server:** Nginx (Automated via bash User Data script)

## Step-by-Step Build Instructions

If you want to build this yourself in the AWS Console, follow these steps in order!

### Step 1: Hire the Bouncers (Security Groups)
1. Go to **EC2 > Security Groups** and create `alb-sg` (The Front Door Bouncer). Add an Inbound Rule for HTTP (Port 80) from `0.0.0.0/0` (Anywhere).
2. Create a second security group called `ec2-private-sg` (The Kitchen Bouncer). Add an Inbound Rule for HTTP (Port 80), but set the source to your `alb-sg`. This ensures the servers only talk to the load balancer!

### Step 2: Set up the Greeter (Target Group & ALB)
1. Go to **Target Groups** and create a group named `web-app-tg`. Choose "Instances" as the target type, but do not register any instances yet.
2. Go to **Load Balancers** and create an **Application Load Balancer** named `web-app-alb`. Set it to Internet-facing, select at least two Availability Zones, and assign it the `alb-sg` security group. Set the listener action to forward traffic to `web-app-tg`.

### Step 3: Create the Cashier Blueprint (Launch Template)
1. Go to **Launch Templates** and create `web-app-template`.
2. Choose **Amazon Linux 2023** (Free Tier eligible) and a **t2.micro** instance type.
3. For network settings, attach the `ec2-private-sg` security group.
4. Scroll down to **Advanced Details** and paste the `user-data.sh` script (found in this repository) into the User Data box. This automatically installs Nginx and creates a custom webpage when the server boots!

### Step 4: Hire the Manager (Auto Scaling Group)
1. Go to **Auto Scaling Groups** and create `web-app-asg`.
2. Attach your `web-app-template`.
3. Attach it to your existing load balancer and target group (`web-app-tg`).
4. Set the capacities: Desired: 2, Minimum: 1, Maximum: 4.
5. Set up a Target Tracking Scaling Policy based on Average CPU Utilization, with a target value of 50%.

## How to Test
If you deploy this architecture, copy the ALB's DNS name into your web browser. Refresh the page a few times, and you will see the `Host ID` change on the screen. This proves the load balancer is successfully bouncing your connection between different servers in the background!
