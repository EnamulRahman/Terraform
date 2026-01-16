Introduction to Variables in Terraform (Simplified) 

Now that we understand the basics of Terraform and how to manage infrastructure, let’s look at variables — a key feature that makes Terraform configurations flexible, reusable, and easier to maintain. 

 
 

What Are Variables in Terraform? 

Variables allow you to parameterize your Terraform configuration. 

Instead of hard-coding values like: 

AMI IDs 

Instance types 

Region names 

You define them as variables, which can be changed easily without modifying the main configuration code. 

 
 

Why Use Variables? 

1. Reusability 

The same Terraform code can be reused across: 

Dev 

Staging 

Production 

You simply change the variable values. 

 
 

2. Cleaner and More Maintainable Code 

Variables prevent large configuration files full of repeated values, making your code: 

Easier to read 

Easier to update 

Less error-prone 

 
 

3. DRY Principle (Important for Interviews) 

Terraform variables help follow the DRY principle: 

DRY = Don’t Repeat Yourself 

Instead of repeating the same AMI ID or instance type in multiple places, you define it once as a variable and reuse it. 

📌 Mentioning DRY in interviews shows good engineering practice. 

 
 

Example Use Case (EC2 Instance) 

When creating an EC2 instance, values such as: 

AMI ID 

Instance type 

are perfect candidates for variables. 

For example: 

If you used the same Ubuntu AMI multiple times 

Or wanted to switch instance types easily 

Variables make this simple and scalable. 

 
 

Where to Define Variables 

You can define variables: 

Inside your main .tf file 

Best practice: in a separate file called variables.tf 

Separating variables: 

Keeps your code organised 

Makes configurations easier to manage 

 
 

Variable Types (High-Level) 

Terraform supports multiple variable types, but the most common and important are: 

Input Variables 

Input variables allow users to pass values into Terraform configurations. 

These values are then referenced inside resource blocks. 

 
 

Referencing Variables (Very Important) 

Defining a variable is only half the job. 
You must also reference it correctly in your configuration files. 

Terraform uses a simple syntax to reference variables, which allows resources to dynamically pick up the values you define. 

📌 This is essential for writing flexible and reusable Terraform code. 

 
 

Summary (Interview-Friendly) 

Variables make Terraform dynamic and reusable 

They prevent hard-coding values 

They support the DRY principle 

Best practice is to define them in variables.tf 

Input variables are the most commonly used 

Variables must be defined and referenced correctly 

  

 

//////////////////////// 

 

Terraform Local Variables (Simplified Explanation) 

Now we’re going to focus on local variables in Terraform — a feature that helps you write cleaner, more maintainable, and more readable code. 

Local variables allow you to define values once and reuse them multiple times, which makes your Terraform configurations easier to manage and understand. 

 
 

What Are Local Variables? 

Local variables are used to store internal values within your Terraform configuration. 

They are: 

Defined once 

Computed or assigned internally 

Reused throughout the configuration 

Unlike input variables, local variables are not provided by the user. They are purely for internal use. 

 
 

Why Use Local Variables? 

1. Reduce Repetition (DRY Principle) 

Local variables help you follow the DRY principle: 

DRY = Don’t Repeat Yourself 

If the same value (like an AMI ID, naming format, or tag) is used multiple times, define it once as a local variable. 

📌 Mentioning DRY in interviews shows strong Terraform and software engineering practices. 

 
 

2. Cleaner and Easier-to-Read Code 

Instead of repeating long values throughout your code, local variables: 

Centralise repeated values 

Make configurations easier to read 

Make updates safer and faster 

 
 

How to Define Local Variables 

Local variables are defined inside a locals block. 

A single locals block can contain multiple related values 

These values are grouped together for clarity 

This is commonly used for: 

AMI IDs 

Naming conventions 

Common tags 

Computed values 

 
 

Example Use Case (AMI ID) 

Instead of repeating the same AMI ID across your configuration: 

You define it once in a locals block 

You reference it wherever needed 

This keeps your EC2 configuration clean and DRY. 

 
 

Referencing Local Variables 

Referencing a local variable is very simple: 

Input variables use: var.variable_name 

Local variables use: local.local_name 

So instead of: 

Referencing an AMI via an input variable 

You can reference it as: 

local.instance_ami 

This clearly shows the value is internal to the configuration. 

 
 

Validating the Configuration 

After adding a local variable: 

Run terraform plan 

If Terraform shows “No changes”, everything is working correctly 

This confirms: 

The local variable is correctly defined 

The value is being passed into the resource properly 

 
 

Key Differences: Input vs Local Variables 

Input Variables 

Local Variables 

Provided by users 

Internal only 

Configurable per environment 

Used for reuse & clarity 

Good for flexibility 

Good for DRY & readability 

 
 

Summary (Interview-Ready) 

Local variables store internal reusable values 

Defined in a locals block 

Referenced using local.name 

Help keep Terraform code DRY 

Improve readability and maintainability 

Commonly used for AMIs, tags, and naming logic 

 

//////////////////// 

 

Terraform Output Variables (Simple Explanation) 

What Are Output Variables? 

Output variables in Terraform are used to display values after a terraform apply has completed. 

They commonly show: 

Resource IDs 

Public or private IP addresses 

DNS names 

Any important information created during deployment 

Output variables are useful for: 

Viewing important values in the terminal 

Passing data to other Terraform configurations or modules 

Feeding values into automation tools or scripts 

 
 

Why Are Output Variables Useful? 

Output variables allow you to: 

Easily retrieve important infrastructure details 

Avoid manually searching in the cloud console 

Share values between Terraform modules 

Support automation and CI/CD workflows 

In short: outputs make Terraform results visible and reusable. 

 
 

How to Define an Output Variable 

Output variables are defined using an output block. 

Each output block includes: 

Name – the identifier for the output 

Description (optional but recommended) 

Value – what Terraform should display 

 
 

Referencing a Resource in an Output 

To output a value from a resource, you reference it using this format: 

resource_type.resource_name.attribute 
 

For an EC2 instance, this looks like: 

Resource type: aws_instance 

Resource name: example 

Attribute: id 

So the value becomes: 

aws_instance.example.id 
 

This tells Terraform exactly which resource and attribute to display. 

 
 

What Happens When You Run Terraform? 

terraform plan 

Shows that an output will be generated 

Does not change infrastructure 

terraform apply 

Executes the configuration 

Displays the output values at the end of the run 

If no infrastructure changes were made, Terraform will still print the output values, which is exactly what you want. 

 
 

How Outputs Are Used in Practice 

Output values can be: 

Copied and used manually (e.g. searching for an instance in AWS) 

Passed into other Terraform modules 

Used by scripts, pipelines, or monitoring tools 

They are designed to make your workflow faster and more efficient. 

 
 

Best Practices for Output Variables 

1. Use Descriptive Names 

Choose clear names so the purpose is obvious. 

✅ Example: instance_id 
❌ Example: output1 

 
 

2. Always Add Descriptions 

Descriptions help: 

Other team members understand your code 

Make configurations easier to maintain 

 
 

3. Output Only Useful Information 

Only expose values that are: 

Needed for automation 

Needed for operational visibility 

Required by other modules 

 
 

4. Secure Sensitive Outputs 

Be careful when outputting: 

Passwords 

Secrets 

Tokens 

Use Terraform’s sensitive = true option to prevent values from being shown in the terminal. 

 
 

Interview-Ready Summary 

Output variables display values after terraform apply 

Defined using an output block 

Used to expose resource information like IDs and IPs 

Help with automation, debugging, and modular Terraform 

Sensitive outputs should be protected 

 

////////////////// 

 

 

Terraform Variable Hierarchy (Easy Explanation) 

Terraform allows you to define variables in multiple ways. When the same variable is defined in more than one place, Terraform follows a strict order of precedence (also called variable hierarchy) to decide which value to use. 

Understanding this hierarchy is very important for real-world usage and interviews. 

 
 

Why Variable Hierarchy Matters 

Variable hierarchy allows Terraform to: 

Stay flexible across environments (dev, staging, prod) 

Override values safely when needed 

Avoid hard-coding values 

Support automation and CI/CD pipelines 

 
 

Variable Precedence (Lowest → Highest) 

1️⃣ Default Values (Lowest Priority) 

Defined inside the variable block using default 

Used only if no other value is provided 

Acts as a fallback value 

📌 Example use case: 

Sensible defaults for testing 

Simple single-user setups 

 
 

2️⃣ .tfvars Files 

Files such as: 

terraform.tfvars 

*.tfvars 

Store multiple variable values in one place 

Commonly used for: 

Environment-specific values 

Modules 

Cleaner configurations 

📌 How Terraform uses them: 

Automatically loads terraform.tfvars 

Other .tfvars files are passed using -var-file 

📌 Takes precedence over: 

Default values 

 
 

3️⃣ TFVAR Environment Variables 

Set using environment variables 

Must be prefixed with: 

TF_VAR_<variable_name> 
 

📌 Example: 

export TF_VAR_instance_type=t2.micro 
 

📌 Key points: 

Overrides defaults and .tfvars files 

Very common in CI/CD pipelines 

Useful for secrets and environment-specific values 

 
 

4️⃣ Command-Line Flags (Highest Priority) 

Passed directly when running Terraform commands 

Uses the -var flag 

📌 Example: 

terraform apply -var="instance_type=t2.micro" 
 

📌 Key points: 

Overrides everything else 

Best for quick, one-off overrides 

Not ideal for long-term configuration storage 

 
 

Full Precedence Order (Easy to Memorise) 

Lowest → Highest 

Default values 

.tfvars files 

TF_VAR_ environment variables 

Command-line -var flags 

👉 Highest precedence always wins 

 
 

Interview-Ready Summary 

Terraform variable hierarchy determines which variable value is used when the same variable is defined in multiple places. Default values have the lowest priority, followed by .tfvars files, then TF_VAR_ environment variables, and finally command-line -var flags, which have the highest precedence. 

 

////////////////////////////////// 

 

Terraform Variable Types (Simple Explanation) 

Terraform supports different types of variables to make your configurations more reusable, dynamic, and flexible. 

When we created our EC2 instance earlier, we used an input variable for the instance type (for example, t2.micro). Instead of hard-coding it, we passed it in as a variable, which is exactly how Terraform is meant to be used. 

Terraform variable types fall into two main categories: 

Primitive types 

Complex types 

To make this easier to understand, think of cooking: 

Primitive types = individual ingredients (flour, sugar, eggs) 

Complex types = full recipes (multiple ingredients grouped together) 

 
 

Primitive Types (Simple Values) 

Primitive types represent single, simple values. 

1️⃣ String 

Represents text 

Written inside quotes 

📌 Example: 

"hello" 

"t2.micro" 

Think of this as: 

A label or name, like the name of an ingredient 

 
 

2️⃣ Number 

Represents numeric values 

Can be whole numbers or decimals 

📌 Examples: 

15 

6.28 

42 

Think of this as: 

The quantity of an ingredient (e.g. 500 grams of flour) 

 
 

3️⃣ Boolean (bool) 

Can only be true or false 

Often used for conditional logic 

📌 Examples: 

true 

false 

Think of this as: 

A yes or no decision 

Is the oven preheated? 

Should we add sugar? 

 
 

Summary of Primitive Types 

Type 

Purpose 

Example 

string 

Text 

"t2.micro" 

number 

Numeric values 

42 

bool 

Yes / No decisions 

true 

 
 

Complex Types (Grouped Values) 

Complex types allow you to group multiple values together, making configurations more powerful and structured. 

These are like full recipes instead of individual ingredients. 

Terraform has three main complex types. 

 
 

1️⃣ List 

An ordered collection 

All values must be the same type 

📌 Example: 

A list of strings 

A list of availability zones 

Think of this as: 

A shopping list 

Flour 

Sugar 

Eggs 

All items are similar and stored in order. 

 
 

2️⃣ Map 

Uses key-value pairs 

Keys map to specific values 

Think of this as: 

A cookbook index 

Flour → 500g 

Eggs → 3 

Sugar → 200g 

This allows you to: 

Look up values using meaningful names 

 
 

3️⃣ Object 

A collection of named attributes 

Each attribute can have a different type 

📌 Example attributes: 

string 

number 

boolean 

Think of this as: 

A full recipe card 

Ingredient name 

Quantity 

Whether it’s required or optional 

An object can combine strings, numbers, and booleans into one structured value. 

 
 

Quick Comparison 

Category 

Description 

Cooking Analogy 

Primitive types 

Single simple values 

Ingredients 

List 

Ordered values of same type 

Shopping list 

Map 

Key-value pairs 

Ingredient amounts 

Object 

Multiple attributes, mixed types 

Full recipe card 

 
 

Interview-Ready Summary 

Terraform variable types are divided into primitive and complex types. Primitive types include strings, numbers, and booleans, which represent single values. Complex types include lists, maps, and objects, which group multiple values together and allow Terraform configurations to be more structured, reusable, and maintainable. 

///////////////////////////////////// 

 

 

 
