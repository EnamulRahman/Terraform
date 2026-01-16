# EC2 Deployment with Cloud-Init and Terraform

This project demonstrates automated EC2 deployment in AWS using **Terraform** and **Cloud-Init**.  
The EC2 instance comes fully configured with **NGINX** and a custom webpage, requiring **zero manual setup**.

---

## 🏗 Architecture

- **Region:** eu-west-2 (London)  
- **VPC:** Custom VPC with public subnet  
- **Internet Gateway:** Provides internet access  
- **Route Table:** Routes traffic to IGW  
- **Security Group:** HTTP open to all; optional SSH  
- **EC2 Instance:** Ubuntu 22.04 LTS, provisioned via Terraform  
- **Cloud-Init:** Automates package installation, NGINX setup, and custom webpage deployment  

**Diagram (Optional)**:  

[Internet] --> [IGW] --> [Public Subnet] --> [EC2 Web Server]

--> [Security Group: HTTP/SSH]



---

## 📁 Project Structure

assignment2/
├── main.tf # Terraform resources (VPC, Subnet, IGW, SG, EC2)
├── variables.tf # Variable definitions
├── outputs.tf # Outputs: instance IP, DNS, URL
├── cloud-init.yaml # Cloud-init configuration for EC2
├── terraform.tfvars # Overrides default variables
└── README.md # Project documentation

yaml


---

## ⚙️ Prerequisites

1. AWS account with permissions to create EC2, VPC, Security Groups  
2. AWS CLI configured (`aws configure`)  
3. Terraform v1.0+ installed  
4. (Optional) SSH key pair in eu-west-2 for SSH access  

---

## ☁️ Cloud-Init Workflow

Cloud-init runs **on first boot** of the EC2 instance and performs the following:

1. Updates the package cache (`apt update`) and upgrades packages  
2. Installs **NGINX**  
3. Creates a custom HTML page at `/var/www/html/index.html`  
4. Starts and enables the NGINX service  
5. Logs completion at `/var/log/cloud-init-output.log`  

This ensures the instance is fully configured without manual intervention.

---

## 🛠 Terraform Workflow

Terraform provisions the infrastructure in a **repeatable and automated** way:

1. **Initialize Terraform**
```bash
terraform init
Validate configuration


terraform validate
Preview plan


terraform plan -var-file="terraform.tfvars"
Apply configuration


terraform apply -var-file="terraform.tfvars"
Get outputs


terraform output website_url
Destroy resources (cleanup)


terraform destroy -var-file="terraform.tfvars"
🌐 Access the Website
After deployment:

Copy the URL from Terraform output:


terraform output website_url
Open it in your browser to see the automated NGINX webpage.

🧩 Terraform Variables
variables.tf defines all configurable parameters:

aws_region → AWS deployment region

instance_type → EC2 instance type

instance_name → Name tag for EC2 instance

key_name → Optional SSH key

enable_public_ip → Public IP toggle

allowed_ssh_cidr → Optional SSH restriction

terraform.tfvars overrides defaults as needed.

🔧 Security Group
Port 80: Open for HTTP

Port 22: Optional SSH access, restricted via allowed_ssh_cidr

Egress: All outbound traffic allowed

📝 Troubleshooting
Website not loading: Wait 2–3 minutes for cloud-init; ensure SG allows port 80; verify public IP

Cloud-init failed: SSH (if key provided) and check logs:


sudo cat /var/log/cloud-init-output.log
sudo systemctl status nginx
Terraform apply fails: Check AWS credentials, region, and IAM permissions

💡 Notes
Using a custom VPC ensures compatibility even if default subnets are missing

Root volume is tagged for clarity

The workflow is reusable for multiple environments (dev/staging/prod)

💰 Cost
t3.micro in eu-west-2 is Free Tier eligible

Destroy resources after use to avoid charges
