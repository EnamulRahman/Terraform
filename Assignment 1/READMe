🚀 Terraform WordPress Deployment on AWS
📌 Project Overview

This project uses Terraform to deploy a complete WordPress stack on AWS from scratch. The infrastructure is fully managed as code and includes:

An EC2 instance running WordPress

Security groups for controlled access

User data (cloud-init) to install Apache, PHP, MySQL, and WordPress

A publicly accessible WordPress site

All resources provisioned and managed via Terraform

This assignment demonstrates how Terraform can manage real-world infrastructure end-to-end.

🏗️ What I Built

Using Terraform, I deployed:

✅ AWS EC2 instance hosting WordPress

✅ Security Groups allowing HTTP (80) and SSH (22)

✅ User data script to:

Install Apache, PHP, MySQL client

Download and configure WordPress automatically

✅ Public endpoint allowing access to the WordPress setup page

✅ Outputs exposing the instance public IP/DNS

Once deployed, navigating to the EC2 public IP loads the WordPress installation screen successfully.

🗂️ Terraform Code Structure
.
├── main.tf        # Provider config, EC2, security groups, user data
├── variables.tf   # Input variables (region, key pair, instance type, etc.)
├── outputs.tf     # Outputs such as public IP and DNS
├── user-data.sh   # Cloud-init script to install WordPress (optional file)
└── README.md

File Breakdown
main.tf

Defines:

AWS provider

EC2 instance resource

Security groups

User data script

variables.tf

Makes the code reusable by parameterising:

AWS region

Instance type

Key pair name

AMI ID

outputs.tf

Displays:

Public IP

Public DNS name

SSH connection command

user-data.sh

Bootstraps the server by installing:

Apache

PHP modules

MySQL client

WordPress

📚 What I Learned

How to provision real AWS infrastructure using Terraform

How user data/cloud-init automates server configuration

How security groups control inbound and outbound traffic

How Terraform manages:

State

Resource dependencies

Idempotent deployments

How to debug failed EC2 provisioning and unhealthy services

How small configuration mistakes can break infrastructure — and how to trace them

🐛 Issues I Hit & How I Fixed Them
❌ WordPress not loading in browser

Cause: Apache wasn’t running due to missing packages
✅ Fixed by correcting the user data script and restarting Apache

❌ EC2 instance unreachable

Cause: Security group missing HTTP (80) rule
✅ Added inbound rule allowing 0.0.0.0/0 on port 80

❌ SSH key authentication failing

Cause: Incorrect key permissions and wrong key pair name
✅ Fixed using:

chmod 400 my-key.pem


and matching the key pair name in Terraform

❌ Terraform apply succeeded but site still broken

Cause: User data script failed silently
✅ Checked /var/log/cloud-init-output.log on the instance and corrected the script

📸 Screenshots

Screenshots of:

Terraform successful apply

EC2 instance running

WordPress setup screen in browser

👉 These are included in the /screenshots folder in this repository.

🧪 How to Run This Project
terraform init
terraform plan
terraform apply


Then visit the outputted public IP in your browser to complete WordPress setup.

To destroy resources:

terraform destroy

✅ Requirements Met
Requirement	Status
EC2 running WordPress	✅
Security groups	✅
User data / cloud-init	✅
Public endpoint	✅
All resources via Terraform	✅
main.tf	✅
variables.tf	✅
outputs.tf	✅
Successful WordPress install	✅
🏁 Final Thoughts

This project gave me hands-on experience deploying real infrastructure using Terraform and taught me how critical networking, bootstrapping, and debugging are in cloud engineering. Debugging issues that took over 2 hours to resolve ended up being caused by simple misconfigurations — a powerful lesson in real-world DevOps work.
