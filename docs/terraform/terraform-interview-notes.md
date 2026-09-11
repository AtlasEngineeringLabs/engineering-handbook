# Terraform Interview Notes

# Module 1 - Architecture


## Core Concepts

- Infrastructure as Code (IaC)
- Declarative vs Imperative
- Desired State
- Terraform State
- Terraform Core
- Provider

## Typical Interview Questions

- What is Infrastructure as Code?

- What problem does Terraform solve?

- Declarative vs Imperative?

- Explain the Terraform architecture.

- What is Terraform State?

- Why is Terraform State required?

## Practical Scenarios

A company currently builds all cloud infrastructure manually through the AWS or GCP console.

How would Terraform improve this process?

## Common Mistakes

- Thinking Terraform creates infrastructure immediately.

- Thinking Terraform State is optional.

- Confusing Desired State with Current State.

- Believing Terraform stores infrastructure.

## Key Terminology

- Infrastructure as Code

- Declarative

- Desired State

- State File

- Provider

- Execution Plan

## Things to Remember

---

## Revision Checklist

Can I explain:

☐ Infrastructure as Code

☐ Declarative

☐ Terraform State

☐ Desired State

☐ Terraform Core

---

# Module 2 - Workflow
   

## Core Concepts

- Terraform workflow is the standard sequence for managing infrastructure as code: write configuration, initialize, plan, apply, and eventually destroy. Each stage builds on the previous one and exists to catch errors early, minimize surprises, and keep infrastructure changes predictable and auditable.


## Typical Interview Questions
   
- What does terraform init do?

- What does terraform plan do?

- What is .terraform?

- What is .terraform.lock.hcl?

- Why use validate before apply?

- What are the core Terraform workflow commands, and what does each do?

- What's the difference between terraform plan and terraform apply?

- Why does terraform init need to run again sometimes?

- What happens if you skip terraform plan and go straight to apply?

- How does Terraform decide the order to create/destroy resources?

- What's stored in the state file, and why does it matter?

- How would you safely remove a single resource without destroying everything?

## Practical Scenarios
   
- A team member adds a new provider or module to the configuration, but the next apply fails with errors about missing plugins.

What Terraform command should be run, and why?

- A teammate manually changes a resource's configuration directly in the GCP console, bypassing Terraform entirely.

How would Terraform detect this, and what command would reveal it?

- A company needs to tear down an entire dev/test environment at the end of each day to save on cloud costs.

Which Terraform command handles this, and what risk should be considered before running it?

- A team wants changes reviewed and approved before they're applied automatically in a CI/CD pipeline.

How would you structure the Terraform workflow to support a plan-then-apply approval step?

- A resource must be created only after a specific GCP API is enabled, but there's no direct attribute reference linking the two resources.

How would you enforce this ordering in Terraform?

- A company currently builds all cloud infrastructure manually through the AWS or GCP console.

How would Terraform improve this process?

## Common Mistakes
   
- Running terraform apply without reviewing terraform plan output first.

- Forgetting to run terraform init after adding a new provider or module.

- Hardcoding project IDs, regions, or zones instead of using variables.

- Overusing depends_on when an implicit dependency (via attribute reference) would work.

- Not committing/sharing state properly, leading to conflicting or stale state across a team.

- Running terraform destroy casually in a shared or production environment.

## Key Terminology
   
State file — Terraform's record of what infrastructure it manages and its last known configuration.

Provider — a plugin (e.g. google) that lets Terraform talk to a specific platform's API.

Plan — a preview of changes Terraform would make to reach the desired state.

Apply — executes the plan and creates/updates/destroys real infrastructure.

Drift — when real infrastructure no longer matches the state file (e.g. due to manual changes).

Idempotency — reapplying the same configuration produces no further changes if nothing's changed.

## Things to Remember
   
- Workflow order: fmt → validate → init → plan → apply (→ destroy when needed).

- init is required whenever providers/modules change or on a fresh clone of the repo.

- plan never changes real infrastructure — it's a dry run.

- Always review plan output before apply, especially for destructive changes (shown as "-" or "replace").

- State should be stored remotely (e.g. GCS backend) with locking when working in a team.

## Revision Checklist

 Can explain what each workflow command does, in order

 Can explain the difference between plan and apply

 Understand when init needs to be rerun

 Can explain implicit vs explicit dependencies

 Understand what the state file is and why it matters

 Know how to safely preview and apply changes in a team/CI setting

 Can name at least 3 common mistakes and how to avoid them

# Module 3 - Providers
   
---

## Core Concepts
   
Provider Plugins

Terraform Registry

Provider Versioning

Authentication

Cloud APIs

## Typical Interview Questions

- What is a Terraform provider, and how does it differ from a module?

- How does Terraform authenticate to a cloud platform like AWS or GCP?

- Why is pinning a provider version important, and what could go wrong if you don't?

- Where does Terraform download providers from, and what are the security implications of that?

- How would you use Terraform in an environment with no direct internet access to the public registry?

- What's the difference between provider-level authentication and resource-level IAM permissions?

- How do you avoid hardcoding credentials in a Terraform configuration?

## Practical Scenarios
   
Your company uses:

AWS

GitHub

Cloudflare

Explain how Terraform manages all three.

A security policy prohibits storing cloud credentials in plaintext files or committing them to version control.

How would you configure provider authentication in Terraform to comply with this?

Your organization operates in an air-gapped or restricted network with no access to the public Terraform Registry.

How would you source and manage providers in this environment?

A provider is upgraded automatically and introduces a breaking change or an unreviewed behavior change into production infrastructure.

How would you have prevented this, and what should have been configured beforehand?

You're asked to review a Terraform configuration for a new cloud project before it goes live.

What would you check regarding the providers being used, in terms of source, version, and permissions?

An engineer configures a provider using a personal access key with broad admin permissions, rather than a scoped service account.

What's the security risk here, and how should provider authentication be structured instead?

Your team needs to audit which external provider plugins are being pulled into the environment and from where.

How would you verify the source and integrity of a provider before allowing it into a pipeline?

## Common Mistakes
   
- Thinking Providers are built into Terraform.

- Thinking Terraform talks directly to AWS.

- Not version pinning Providers.

- Not pinning provider versions, allowing unreviewed upgrades to introduce breaking or insecure changes.

- Hardcoding access keys/secrets directly in .tf files instead of using environment variables, a secrets manager, or workload identity.

- Granting providers overly broad IAM permissions instead of following least privilege.

- Downloading providers from untrusted or unverified sources.

- Ignoring provider lock file (.terraform.lock.hcl) changes during code review.

- Assuming provider authentication is a one-time setup rather than something to rotate/review periodically.

## Key Terminology
   
- Provider — a plugin that lets Terraform communicate with a specific platform's API (e.g. google, aws, azurerm).

- Terraform Registry — the public (or private) source Terraform downloads providers and modules from.

- Provider Versioning — pinning a provider to a specific version or range to control when upgrades happen.

- Lock File (.terraform.lock.hcl) — records exact provider versions and checksums used, ensuring consistent, verified installs.

- Authentication — how a provider proves identity to the target platform (API keys, service accounts, workload identity, environment credentials).

- Least Privilege — granting only the minimum permissions a provider/service account needs to manage its resources.

## Things to Remember
   
- Providers are separate plugins from Terraform core — they must be downloaded via terraform init.

- Always pin provider versions (e.g. version = "~> 5.0") to avoid unreviewed breaking changes.

- Commit the lock file to version control so every team member and pipeline uses identical, verified provider versions.

- Prefer credential-less or short-lived authentication methods (e.g. workload identity federation, IAM roles) over static keys.

- Never commit credentials to source control — use environment variables, secret managers, or CI/CD secret stores.

- Private/self-hosted registries or mirrors can be used in restricted network environments.

## Revision Checklist

- Can explain what a provider is and how it differs from a module

- Understand where providers come from and how they're verified (registry, lock file, checksums)

- Can explain secure authentication options (service accounts, workload identity vs static keys)

- Understand why and how to pin provider versions

- Can identify insecure credential-handling practices in a Terraform config

- Know how to operate Terraform in a restricted/air-gapped network

 Understand least-privilege principles as applied to provider authentication

# Module 4 - Resources
   
---

## Core Concepts
   
- Resource Blocks

- Resource Types

- Resource Names

- Arguments

- Attributes

- References

- Implicit Dependencies

- Explicit Dependencies

- Dependency Graph

## Typical Interview Questions
   
- What is a Terraform Resource?

- Resource Type vs Resource Name?

- Argument vs Attribute?

- What is an implicit dependency?

- When should depends_on be used?

- How does Terraform determine execution order?

## Practical Scenarios

- A Virtual Machine depends on:

- Network

- Subnet

- Firewall

How does Terraform determine the build order?

## Common Mistakes
   
- Thinking Terraform executes top-to-bottom.

- Confusing the Terraform Resource Name with the Cloud Resource Name.

- Using depends_on unnecessarily.

- Hardcoding values instead of using references.

## Key Terminology
   
- Resource Block — the resource "<TYPE>" "<NAME>" { } syntax used to declare a piece of infrastructure.

- Resource Type — the kind of infrastructure being managed (e.g. google_compute_instance), defined by the provider.

- Resource Name — the local Terraform identifier used to reference the resource within the configuration only.

- Argument — an input value configured inside a resource block to set its desired state.

- Attribute — a value exposed by a resource, either computed after creation (e.g. id, self_link) or derived from an argument.

- Reference — using one resource's attribute elsewhere in the config (e.g. google_compute_network.vpc.self_link), which links resources together.

- Implicit Dependency — a dependency Terraform infers automatiesource reference.

- Explicit Dependency (depends_on) — a manually declared dependency used when no direct reference exists to infer order from.

- Dependency Graph — the internal graph Terraform builds from references and depends_on to determine safe creation/destruction order.

## Things to Remember
   
- Terraform does not execute resources top-to-bottom — order is determined entirely by the dependency graph.

- A resource reference (e.g. passing a network's self_link into a subnet) creates an implicit dependency automatically — no extra config needed.

- Use depends_on only when there's a real dependency with no direct attribute reference to infer it from (e.g. waiting on an enabled API).

- The Resource Name is a Terraform-only identifier — it has no bearing on the actual name Terraform assigns the resource in the cloud platform.

- Resources without any dependency relationship can be created in parallel, which is why the dependency graph — not file order — controls execution.

- Prefer references over hardcoded values so Terraform can track dependencies and update related resources automatically when something changes.

## Revision Checklist

☐ Explain Resource Blocks

☐ Explain Resource Types

☐ Explain Resource Names

☐ Explain Arguments

☐ Explain Attribudency Graph


----------------------------------

# Interview Tips

## Explain Concepts

Always explain:

What it is

Why it exists

When to use it

Trade-offs

Real-world example
