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







