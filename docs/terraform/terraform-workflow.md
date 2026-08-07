The Terraform lifecycle.

High Level:

Write Configuration (.tf)
          │
          ▼
terraform fmt
          │
          ▼
terraform validate
          │
          ▼
terraform init
          │
          ▼
terraform plan
          │
          ▼
terraform apply
          │
          ▼
Infrastructure Exists

The purpose of each command.
Command			Purpose
terraform fmt		Format configuration files consistently - Before Terraform even thinks about infrastructure, it can format your configuration. 
This checks:
Consistent style
Easier reviews
Cleaner Git diffs
Standard formatting across teams

terraform validate	Check configuration syntax and internal consistency
This checks whether your configuration is structurally valid. Validation catches many problems before Terraform even attempts to build a plan. It's a fast, local quality check.

terraform init	Initialise the working directory and download providers. Prepares the working directory so Terraform has everything it needs to operate. Itructure.

Project Directory

↓

Read Configuration

↓

Identify Providers

↓

Download Provider Plugins

↓

Create .terraform/

↓

Create .terraform.lock.hcl

↓

Ready

terraform plan	Calculate the changes required
Reads your configuration.
Reads the state file.
Queries the provider about the actual infrastructure.
Compares the desired state with reality.
Produces an execution plan.

terraform apply	Execute the approved plan
	•	Executes the planned changes.
	•	Calls the provider.
	•	The provider calls the cloud APIs.
	•	Resources are created, modified, or destroyed.
	•	The state file is updated.
Now the infrastructure matches the desired configuration.

terraform destroy	Remove managed infrastructure
Terraform simply calculates a different desired state. 
It then creates a plan to remove the managed resources.

So destroy is essentially another planning and execution cycle, but with the goal of deleting infrastructure rather than creating it.

The .terraform directory. it, you'll see this directory. This contains downloaded provider plugins and other working files Terraform needs.
Think of it as a local cache. You generally do not commit this directory to Git because it can be recreated by running terraform init again.

The .terraform.lock.hcl file. This file records the exact provider versions Terraform selected. The lock file helps ensure everyone is using the same provider versions, improving consistency across machines and CI/CD pipelines.

A section titled "Professional Workflow" describing how teams typically use fmt, validate, plan, PR reviews, and apply.

Terraform Flow & Lifecycle

Developer
     │
     ▼
Write Terraform Configuration
     │
     ▼
terraform fmt
     │
     ▼
terraform validate
     │
     ▼
terraform init
     │
     ▼
Download Providers
     │
     ▼
terraform plan
     │
     ▼
Compare:
- Configuration
- State
- Cloud
     │
     ▼
Execution Plan
     │
     ▼
terraform apply
     │
     ▼
Cloud Infrastructure
     │
     ▼
Update State

Real-World Engineering WorkFlow

Developer

↓

Write Code

↓

terraform fmt

↓

terraform validate

↓

terraform plan

↓

Commit

↓

Pull Request

↓

CI/CD runs fmt + validaey Takeaways

Key Takeaways

If you remember only six things from today's module:
	1	fmt keeps configuration consistent.
	2	validate checks your configuration before planning.
	3	init prepares the working directory by downloading providers and creating local metadata.
	4	plan calculates changes without making them.
	5	apply executes the approved plan and updates the state.
	6	destroy is simply a plan whose desired end state is no managed infrastructure.

