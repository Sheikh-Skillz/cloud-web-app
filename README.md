# cloud-web-app
This project focuses specifically on Amazon Elastic Compute Cloud (EC2) and teaches students how to transition from a single vulnerable server to a production-ready, fault-tolerant fleet. It accommodates both skill levels in your class: beginners configure the EC2 instances, User Data scripts, and security controls, while intermediates build the automated scaling, health-checking, and load-balancing layer.


Step 1 — Create the Launch Template
1. Open EC2
Sign in to the AWS Console.
In the search bar at the top, type EC2.
Select EC2.
Confirm the Region in the upper-right corner.
2. Create the Launch Template

In the left navigation:

Instances → Launch Templates

Then:

Click Create launch template.
For Launch template name, enter:

![image alt](https://github.com/Sheikh-Skillz/cloud-web-app/blob/5a75e1be6bbe3fc726599af9435d8ae992a89273/create-launch-template.png
)

Choose the AMI

Under Application and OS Images (Amazon Machine Image):

Select Amazon Linux.
Choose Amazon Linux 2023 AMI.
Make sure it is the 64-bit x86 version unless your instructor specifically wants ARM.
4. Choose instance type

Under Instance type, select:

t3.micro

![image alt](https://github.com/Sheikh-Skillz/cloud-web-app/blob/23e6175ba7a53e0328b5640b03808cf6217bb890/choose-the-AMI.png)


Network settings

Find Network settings.

For Subnet, don't permanently select a specific subnet if you are going to use the Launch Template with an ASG across multiple AZs.

Set:

Subnet → Don't include in launch template

We'll select the subnets when creating the ASG.

For Firewall / Security groups, we'll create the EC2 security group first.

Click Create security group if the console gives you that option.

Use:

Security group name
cloud-web-ec2-sg

Description
Allow HTTP only from ALB

For inbound rules, you'll eventually want:

Type	Port	Source
HTTP	80	ALB security group

![image alt](https://github.com/Sheikh-Skillz/cloud-web-app/blob/c8716e886487edde86711f87116d32aefb531f43/create-ec2-security-group.png)


Advanced details

Scroll down and expand:

Advanced details

Find:

User data

Paste this:

![image alt](https://github.com/Sheikh-Skillz/cloud-web-app/blob/0650c18b3f706f52d84d4289c5371d89872aae45/add-user-data.png)


Create the template

Scroll to the bottom and click:

Create launch template

You should now see:

cloud-web-template

in your Launch Templates list.



This is called security-group chaining.

2A. Create the ALB security group

Go to:

EC2 → Security Groups

Click:

Create security group

Enter:

Security group name
cloud-web-alb-sg

Description
Allow public HTTP traffic to ALB

Select your VPC.

Under Inbound rules, click Add rule.

Configure:

Field	Value
Type	HTTP
Port	80
Source	Anywhere-IPv4
CIDR	0.0.0.0/0

For outbound rules, leave the default:

All traffic → 0.0.0.0/0

Click:

Create security group




Step 3 — Create the Target Group and ALB
3A. Create the Target Group

In the EC2 console, find:

Load Balancing → Target Groups

Click:

Create target group

Choose target type

Select:

Instances

Click Next.

Basic configuration

Set:

Target group name

cloud-class-web-tg

Protocol

HTTP

Port

80

For the VPC, select the same VPC used by your Launch Template.

Health checks

Leave:

Health check protocol: HTTP

Health check path:

/

Click:

Next

Click:

Create target group

![image alt](https://github.com/Sheikh-Skillz/cloud-web-app/blob/325c634043d9d3ce19680e1a14845a9e8bfa556e/create-target-group.png)



Create the Application Load Balancer

Go to:

EC2 → Load Balancers

Click:

Create Load Balancer


![](https://github.com/Sheikh-Skillz/cloud-web-app/blob/a50a9da1a79e3ca911d55954494f625b2453e358/create-application-load-balancer.png)

You'll see several types.

Select:

Application Load Balancer

Click:

Create

Basic configuration

Load balancer name:

cloud-class-alb

Scheme:

Internet-facing

IP address type:

IPv4

Click Next.

Network mapping

Select your VPC.

You need at least two Availability Zones.

For example:

Availability Zone A
└── Public Subnet 1

Availability Zone B
└── Public Subnet 2

Select the first AZ and its public subnet.

Then select the second AZ and its public subnet.

Important

The ALB needs to be placed in public subnets because it needs to receive traffic from the Internet.

You should see something similar to:

AZ	Subnet
...a	Public subnet A
...b	Public subnet B

Don't choose two subnets from the same AZ.

![](https://github.com/Sheikh-Skillz/cloud-web-app/blob/a50a9da1a79e3ca911d55954494f625b2453e358/create-load-balancer-internet-facing.png)




Security groups

For Security groups, select:

cloud-class-alb-sg

Remove any automatically selected security group that you don't want the ALB using.

Listener and routing

Under Listeners and routing:

Protocol: HTTP

Port: 80

For Default action, choose:

Forward to

cloud-class-web-tg

Then click:

Create load balancer

![](https://github.com/Sheikh-Skillz/cloud-web-app/blob/ef1508cf154d9c3edb0168fe9bfc8b6b8f3926fa/click-create-load-balancer.png)


AWS will take a little time to provision it.

Wait until the ALB status becomes:

Active

Step 4 — Create the Auto Scaling Group

Now we're going to connect everything.

Go to:

EC2 → Auto Scaling Groups

Click:

Create Auto Scaling group

![](https://github.com/Sheikh-Skillz/cloud-web-app/blob/ef1508cf154d9c3edb0168fe9bfc8b6b8f3926fa/create-auto-sacle-group.png)


4A. Choose the Launch Template

For Auto Scaling group name, enter:

cloud-class-asg

Under Launch template:

Select:

cloud-class-web-template

Use the latest version.

Click:

Next

'
'
'
'
'
'
'
'
'
'
'
'
'


![](https://github.com/Sheikh-Skillz/cloud-web-app/blob/6669d6d6b94cf112583abf85e200b46474bf6e96/welcome-to-cloud-class.png)

























