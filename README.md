# Scalable Web App (AWS ALB + Auto Scaling) 🍔🚀

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

## How to Test
If you deploy this architecture, copy the ALB's DNS name into your web browser. Refresh the page a few times, and you will see the `Host ID` change on the screen. This proves the load balancer is successfully bouncing your connection between different servers in the background!