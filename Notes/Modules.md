Terraform Modules – Introduction 

As your Terraform code grows and your infrastructure becomes more complex, organizing and reusing your configurations becomes essential. This is where Terraform Modules come in. 

In the next lessons, we will cover: 

What Terraform Modules are 

Why we use Modules 

How to structure Modules properly 

What makes a good Terraform Module 

A hands-on demo of creating and deploying a Module 

 
 

What Is a Terraform Module? 

A Terraform Module is a collection of Terraform configuration files grouped together to perform a specific task. 

Think of a Module as a blueprint for building a piece of infrastructure. 

Examples: 

A single EC2 instance 

A VPC with subnets and route tables 

A standard S3 bucket configuration 

A complete application stack 

Important Concept (Often Missed) 

You’ve actually been using Modules this entire time. 

👉 Any Terraform configuration inside a folder is automatically treated as a Module. 
This is known as the root module. 

 
 

Why Do We Use Terraform Modules? 

There are four main reasons we use Modules: 

1️⃣ Reusability (DRY Principle) 

Modules allow you to write infrastructure code once and reuse it multiple times. 

This follows the software engineering principle: 

DRY – Do Not Repeat Yourself 

Example: 

You create an EC2 instance configuration once 

Turn it into a Module 

Reuse it across multiple projects, environments, or AWS accounts 

 
 

2️⃣ Organization 

As infrastructure grows, Terraform files can become large and difficult to manage. 

Modules help you: 

Break complex infrastructure into smaller, logical pieces 

Keep code clean and readable 

Separate concerns (networking, compute, storage, etc.) 

This is especially important in large-scale environments. 

 
 

3️⃣ Consistency 

Using the same Module across environments ensures: 

The same security rules 

The same naming conventions 

The same configurations from dev → staging → production 

This reduces errors and configuration drift. 

 
 

4️⃣ Collaboration 

Modules make teamwork much easier. 

Instead of everyone writing Terraform from scratch: 

Teams share a common set of approved Modules 

Best practices are built in 

Less Terraform knowledge is required for other teams 

This is very common in DevOps roles, where you might work with: 

Data engineers 

Application developers 

Platform teams 

They can use your Modules to deploy infrastructure safely and consistently. 

 
 

Simple Analogy: Lego Blocks 🧱 

Think of Terraform Modules like Lego blocks. 

Instead of building every house, car, or tree from scratch: 

You create reusable Lego pieces 

Snap them together to build bigger systems 

Terraform Modules work the same way: 

Build once 

Reuse everywhere 

Combine Modules to create complex infrastructure 

 
 

Interview-Ready Summary 

Terraform Modules are reusable collections of Terraform configuration files that define a specific piece of infrastructure. They help enforce the DRY principle, improve code organization, ensure consistency across environments, and enable better collaboration across teams. Every Terraform configuration directory is a Module, with the main one being called the root module. 

//////////////////////////////////////////// 

 

What Makes a Good Terraform Module? 

A good Terraform module is like a well-designed Lego block: 

It fits into many builds 

It’s easy to use 

It’s flexible and reusable 

It does one job well 

This topic is very common in Terraform interviews, especially for junior to senior roles, so understanding this properly will help you stand out. 

 
 

Core Characteristics of a Good Terraform Module 

1️⃣ Flexibility (No Hardcoding) 

A good module avoids hardcoding values that change between environments. 

❌ Bad: 

Hardcoded AMI IDs 

Hardcoded instance types 

Hardcoded regions 

✅ Good: 

Use input variables for values like: 

Instance type 

AMI ID 

Region 

Environment (dev / prod) 

This allows the module to be reused across: 

Multiple environments 

Multiple accounts 

Multiple projects 

 
 

2️⃣ Clear & Useful Outputs 

Modules should expose useful output values so other parts of Terraform can use them. 

Examples: 

EC2 instance ID 

Public or private IP 

Security group ID 

Load balancer DNS name 

Why this matters: 

Other modules or root configurations can consume these values 

Makes modules composable and reusable 

 
 

3️⃣ Well Documented 

Documentation is non-negotiable for good modules. 

A good module should include: 

A README.md 

Clear explanation of: 

What the module does 

Required and optional inputs 

Output values 

Any assumptions or limitations 

Well-documented modules: 

Are easier to share 

Reduce onboarding time 

Improve collaboration across teams 

 
 

4️⃣ Single Responsibility (Modularity) 

Each module should focus on one responsibility only. 

Example: 

An EC2 module → creates EC2 instances only 

A VPC module → networking only 

❌ Avoid: 

Mixing EC2, databases, load balancers, and networking in one module 

Why this matters: 

Easier to reuse 

Easier to test 

Easier to maintain 

Fewer unwanted dependencies 

 
 

5️⃣ Reusability (DRY Principle) 

A good module follows: 

DRY – Do Not Repeat Yourself 

You define infrastructure once, then reuse it: 

Across dev / staging / prod 

Across teams 

Across projects 

This is one of the biggest benefits of Terraform modules and a key interview talking point. 

 
 

6️⃣ Testing & Versioning 

Good modules should: 

Be tested across different environments 

Be versioned to allow safe upgrades 

Why versioning matters: 

Prevents breaking changes 

Allows teams to upgrade gradually 

Makes modules safer in production environments 

 
 

Interview-Ready Summary 

A good Terraform module is flexible, reusable, well-documented, and focused on a single responsibility. It avoids hardcoded values by using input variables, exposes useful outputs, supports versioning, and is easy for other teams to consume. Following these principles ensures consistency, scalability, and maintainability across environments. 

////////////////////////////////////////////////////////////////////////////////////////////////// 

 

Deploying Our First Terraform Module (Step by Step) 

Now that we understand what modules are, let’s actually deploy our first Terraform module. 

1️⃣ Current Setup (Before Modules) 

Inside our repository: 

We already have an EC2 configuration 

This includes: 

An EC2 instance we created earlier 

A resource we imported into Terraform 

All of this code worked previously and is unchanged for now 

We also have: 

A provider file with: 

Terraform block 

Remote backend (S3) 

AWS provider 

The only new addition is: 

region = eu-west-1 
This was added directly in the provider block for consistency, instead of using an environment variable. 

We also have: 

variables.tf → input variables 

locals.tf 

outputs.tf 

terraform.tfvars → demonstrates variable hierarchy and precedence 

So far, everything should look familiar. 

 
 

2️⃣ Why We’re Modularising 

At the moment: 

All EC2 resources live directly in the root configuration 

This is fine for learning, but not scalable 

Our goal: 

Follow modularity 

Deploy EC2 instances using a reusable module 

Keep the root configuration clean 

 
 

3️⃣ Preparing for Modularisation 

Before converting to a module: 

It’s a good idea to delete existing EC2 instances 

This ensures Terraform will actually create resources when we run plan 

 
 

4️⃣ Creating the Module Structure 

Terraform modules follow a simple folder structure. 

At the root of the repo: 

Create a folder called modules 

Inside modules: 

Create a folder called EC2 

This gives us: 

modules/ 
  EC2/ 
 

This structure allows us to add more modules later (VPC, RDS, ALB, etc.). 

 
 

5️⃣ Moving EC2 Code into the Module 

Inside modules/EC2: 

Move ec2.tf 

Move variables.tf 

Now: 

All EC2-related logic lives inside the module 

The module contains: 

Resource definitions 

Input variables 

What stays outside the module: 

Provider configuration 

Backend configuration 

terraform.tfvars 

Why? 

These are not reusable 

A module should be: 

Simple 

Flexible 

Driven by variables 

Ideally, even tags should be variables (we can improve that later). 

 
 

6️⃣ What Happens If We Run Terraform Now? 

If we run: 

terraform init 
terraform plan 
 

Terraform shows: 

2 resources to destroy 

Why is this happening? 

Because: 

Terraform no longer sees any resources in the root configuration 

The EC2 resources exist only as a module template 

But the module is not being called 

From Terraform’s perspective: 

Desired state = “no resources” 

Current state = “EC2 instances exist” 

Result = destroy them 

This is expected behaviour. 

 
 

7️⃣ Calling the Module (Important Step) 

Modules must be called from the root module. 

Best practice: 

Create a main.tf file in the root directory 

Inside main.tf, define a module block: 

Give the module a name (e.g. ec2) 

Specify the source (this is required) 

Example (conceptually): 

The source points to ./modules/EC2 

This can be a relative or absolute path 

At this point: 

Terraform now knows: 

The module exists 

The module should be used 

 
 

8️⃣ Re-initialising Terraform 

Run: 

terraform init 
 

Why? 

It initializes: 

Providers 

Backend 

Modules 

Terraform now downloads and prepares the local EC2 module. 

 
 

9️⃣ Running Terraform Plan Again 

Now run: 

terraform plan 
 

This time: 

Output changes from “2 to destroy” → “2 to add” 

Terraform now sees: 

The module 

The EC2 resources defined inside it 

This is exactly what we want. 

 
 

🔟 Applying the Changes 

Run: 

terraform apply 
 

What happens: 

Old EC2 instances are destroyed 

New EC2 instances are created via the module 

Why is Terraform recreating them? 

The resource address has changed 

Previously: 

aws_instance.example 
 

Now: 

module.ec2.aws_instance.example 
 

Terraform treats this as: 

Old resource → delete 

New resource → create 

 
 

⚠️ Important Real-World Warning (Production) 

In production: 

You do NOT want Terraform to destroy live infrastructure 

To prevent this: 

Use terraform state mv 

This moves the resource in state from: 

Old address → new module address 

No destruction occurs 

This is more advanced, but very important in real jobs. 

 
 

✅ Final Takeaway 

Modules are templates until they are called 

Terraform compares desired state vs current state 

Moving resources into a module changes their state address 

Destruction during modularisation is expected unless state is moved 

Always review terraform plan, especially in production 

 
