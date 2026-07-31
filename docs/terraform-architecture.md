Before Terraform

Engineer
    │
    ▼
Login to Google Cloud Console
    │
    ▼
Create VPC
    │
    ▼
Create Firewall
    │
    ▼
Create VM
    │
    ▼
Configure Network
    │
    ▼
Install Software

Infrastructure as Code (IaC) mean Infrastructure is defined in code instead of being created manually.

This makes the process easerliy repeatable

The process uses:

Cloud providers expose APIs that allow resources to be created programmatically.

Terraform provides a consistent way to use those APIs.

Instead of learning a different workflow for every cloud provider, you learn one approach that works across many providers.

High-Level Architecture

          Terraform Configuration (.tf)
                    │
                    ▼
            Terraform Core Engine
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
   Google       AWS        Azure
   Provider    Provider    Provider
        │           │           │
        ▼           ▼           ▼
     Google       AWS        Azure
       APIs        APIs        APIs
        │           │           │
        ▼           ▼           ▼
 Cloud Resources  Cloud Resources

Its Core Components are:

Terraform
│
├- Configuration
├─Core Engine
├── Providers
└── State

1. Configuration
These are your .tf files.
They describe:
Networks
Virtual machines
Storage
Firewalls
IAM
Databases
They answer one question:
What should exist?

2. Terraform Core
Terraform Core is the brain.
It:
Reads your configuration
Builds a dependency graph
Compares configuration with the current infrastructure
Calculates required changes
Executes those changes in the correct order
You never interact with Terraform Core directly—it runs behind the scenes.

3. Providers
Providers allow Terraform to communicate with external systems.
Examples:
Google Cloud
AWS
Azure
Kubernetes
Docker
GitHub
Cloudflare
Each provider understands the API of the platform it manages.

4. State
State is Terraform's memory.
It records:
What resources Terraform manages
Their IDs
Their current attributes
The relationship between your configuration and the real infrastructure
Without state, Terraform wouldn't know what already exists.

Declarative
Terraform is declarative.
You describe the desired end state.
I want:

- 1 VPC
- 2 Subnets
- 1 Firewall
- 3 Virtual Machines
Terraform decides:
what to create,
in what order,
and how to reach that desired state.
You describe the destination; Terraform plans the route.

The Terraform Workflow
Every Terraform deployment follows the same lifecycle.
Write Configuration
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
Infrastructure Created

Key Takeaways
If you remember only five things from today's lesson, make them these:
Infrastructure as Code means infrastructure is defined in code rather than created manually.
Terraform is declarative—you describe the desired state, not the individual steps.
Terraform itself doesn't talk directly to cloud services; it uses providers.
Terraform Core is responsible for planning and coordinating changes.
State is Terraform's memory of the infrastructure it manages.
