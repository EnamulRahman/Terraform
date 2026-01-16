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

