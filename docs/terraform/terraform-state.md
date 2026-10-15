# Module 7 - Terraform-State
   
---

## Core Concepts
Terraform state is a file (terraform.tfstate) that records which real infrastructure Terraform manages and its last known configuration. It maps resources in your .tf code to real-world objects, lets Terraform calculate what needs to change during plan, and stores resource metadata and dependency information.

Example: after applying google_compute_instance.vm, state records that this block maps to a specific VM ID in GCP. On the next plan, Terraform compares your code, state, and the real VM to decide what (if anything) to change.

   Terraform uses state to maintain the mapping between a Terraform resource address and the corresponding real infrastructure object. It can then use the provider to read the remote object and compare that with the desired configuration.

┌─────────────────────────────┐
│ 1. CONFIGURATION            │
│ What you WANT               │
│                             │
│ main.tf                     │
│ machine_type = "e2-medium"  │
└──────────────┬──────────────┘
               │
               │
               ▼
        ┌──────────────┐
        │  TERRAFORM   │
        │     CORE     │
        └──────┬───────┘
               │
       ┌───────┴────────┐
       ▼                ▼
┌──────────────┐  ┌─────────┘

What is inside the state file

Resource addresses
        │
        ├── google_compute_instance.web
        └── google_compute_network.main

Remote resource identifiers
        │
        └── projects/.../instances/prod-web

Resource attributes
        │
        ├── machine_type
        ├── zone
   formation

Resource metadata / relationships


---

## Typical Interview Questions
 	•	What is Terraform state, and why does Terraform need it?
	•	What happens if you lose or delete the state file?
	•	What is the difference between local and remote state?
	•	What is state locking, and why does it matter?
	•	What is drift, and how does Terraform detect it?
	•	How do you bring existing, manually created infrastructure under Terraform management?
	•	Why shouldn’t state files be committed to version control?
	•	How would you recover from a corrupted state file?

---

## Practical Scenarios
   Two engineers run terraform apply against the same environment at the same time.
What mechanism prevents conflicting changes, and what happens when the second engineer’s run starts?

A teammate manually changes a firewall rule in the GCP console after Terraform deployed it.
How does Terraform detect this, and what happens on the next plan/apply?

A company has existing VMs and networks built manually in the console and wants Terraform to manage them goingyou bring these resources under Terraform control without recreating them?

A team stores terraform.tfstate locally on one engineer’s laptop, and that laptop fails.
What risks does this create, and how should state have been stored instead?

An engineer accidentally commits terraform.tfstate to a public Git repository.
What’s the security risk, and what should be done?

---

## Common Mistakes

   	•	Storing state locally in a team environment, causing conflicts and risk of loss.
	•	Committing state files to version control — they can contain secrets (passwords, keys, IPs) in plaintext.
	•	Manually editing the state file instead of using terraform state commands.
	•	Not enabling state locking on remote backends, allowing concurrent applies to corrupt state.
	•	Ignoring drift and assuming code always reflects real infrastructure.
	•	Not enabling versioning or backups on the remote state bucket.
	•	Deleting state without understanding that Terraform then “forgets” the resources and may try to recreate them.

---

## Key Terminology
	•	State Mapping: the link between a resource block in your config (e.g. google_compute_instance.vm) and the real-world resource ID it manages. Example: google_compute_instance.vm maps to projects/my-proj/zones/us-central1-a/instances/web-server.
	•	Drift: when real infrastructure no longer matches state/configuration, typicalExample: someone resizes a VM in the console. terraform plan (via refresh) detects the difference. terraform plan -refresh-only shows drift without proposing config changes.
	•	Remote State: state stored in a shared, durable backend rather than on a local disk.

 Example:

  terraform {
    backend "gcs" {
      bucket = "my-tf-state-bucket"
      pre= "prod"
    }
  }

	•	Locking: a mechanism preventing multiple operations from modifying state simultaneously. Example: the GCS backend locks automatically during apply; a second concurrent run fails with a “state locked” error until the first completes. If a lock is stuck, terrafe-unlock <LOCK_ID> releases it (use cautiously).
	•	State Security: state can contain sensitive values in plaintext, so it must be protected. Example: store in a GCS bucket with encryption at rest, restricted IAM access, uniform buclevel access, and no public access. Never commit it to Git.
	•	Import: brings existing infrastructure under Terraform management by adding it to state. Example:

  terraform import google_compute_instance.vm projects/my-proj/zones/us-central1-a/instances/web-server

You must also write the matching resource k (or use an import block in newer Terraform versions).
	•	Backup and Recovery: protecting state against loss or corruption.
	Example: enable object versioning on the GCS state bucket so previous versions can be restored. Terraform also creates a local terraform.tfstate.backup for local backends.


---

## Things to Remember
   what happens if state becomes corrupted & how to recover from this.

	•	Terraform loses track of what it manages. On the next plan, it may think nothing exists and propose recreating everything, risking duplicate or conflicting resources.
	•	A partially written or invalid state can cause errors on every command.
How to recover:
	1	Restore from a previous version. With remote state and versioning enabled (e.g. GCS bucket versioning), restore the last known good version of the state object.
	2	Use the local backup. For local backends, restore terraform.tfstate.backup.
	3	Pull and inspect. terraform state pull to view current state; terraform state list to see what’s tracked.
	4	Rebuild with import. If no backup exists, re-import each resource with terraform import (or import blocks), then run terraform plan to confirm there’s no unexpected diff.
	5	Fix targeted problems with state commands. Use terraform state rm (remove a stale entry) or terraform state mv (rename/move) ratheand-editing JSON.
	6	Verify. Run terraform plan and confirm it shows no unexpected changes before any apply.
Other points:
	•	Remote state with locking and versioning is the default for any team setup.
	•	State may contain secrets, so treat it as sensitive data.
	•	Never hand-edit the state file; use terraform state subcommands.
	•	pr modifies infrastructure, but apply does, so verify state health before applying after any recovery.


Terraform state commands
Command	Purpose
terraform state list	List resources recorded in state
terraform state show	Inspect a particular managed resource
terraform state mv		Change a resource's address in state
terraform state rm		Stop tracking a resource without deleting it
terraform state pull	Retrieve the current state snapshot
terraform state push	Write a state snapshot — potentially dangerous
terraform import		Associate an existing resource with a Terraform address

---

## Revision Checklist

	•	Can explain what state is and why Terraform needs it
	•	Can explain state mapping between config and real resources
	•	Understand drift and how plan/-refresh-only reveals it
	•	Can explain local vs remote state and why remote is preferred for teams
	•	Understand locking and how to handle a stuck lock
	•	Know why state is sensitive and how to secure it
	•	Can explain how terraform import works and its limitations
	•	Know how to back up state (versioning) and restore it
	•	Can describe the recovery steps for corrupted or lost state
	•	Know the terraform state subcommands (list, show, mv, rm, pull)


——

##Production Troubleshooting and State Recovery

State lock stuck (“Error acquiring the state lock”) 
Cause: a previous run crashed, was cancelled, or is still running. 
Fix: confirm no other run is active (check CI/CD and teammates), then terraform force-unlock <LOCK_ID>. Never force-unlock while an apply is genuinely in progress.

State corrupted or accidentally deleted 
Fix: restore the last good version from bucket versioning (GCS/S3) or terraform.tfstate.backup for local state. Run terraform plan afterwards to confirm no unexpected changes. If no backup exists, rebuild with terraform import.

Plan wants to recreate or destroy resources unexpectedly 
Causes: lost or wrong state, wrong workspace/backend, renamed resource blocks, or a provider version change. 
Fix: check you’re pointed at the correct backend and workspace, use terraform state list to confirm resources are tracked, and use terraform state mv for renamed resources (or a moved block) instead of letting Terraform destroy and recreate.

Drift detected 
Fix: run terraform plan -refresh-only to see differences. Then either revert the manual change by applying, or update the code to match the intended change. Investigate who changed it and why.

Resource exists in cloud but not in state (“already exists” error) 
Fix: terraform import the resource (or use an import bln plan.

Resource deleted manually but still in state 
Fix: terraform plan will propose recreating it. If it should not be recreated, run terraform state rm <address>.

Wrong backend or workspace 
Symptom: plan shows everything as new. Fix: verify terraform workspace show and the backend config, then ruaform init -reconfigure or -migrate-state as appropriate.

Provider or version mismatch after upgrade 
Fix: check .terraform.lock.hcl, pin provider versions, re-run terraform init -upgrade deliberately, and review plan output carefully before applying.

Recovery best-practice workflow
	1	Stop all pipelines and applies touching that state.
	2	Back up the current state (terraform state pull > backup.tfstate).
	3	Diagnose with terraform state list, terraform state show, and terraform plan.
	4	Restore from a versioned backup if available; otherwise import.
	5	Verify terraform plan shows no unexpected changes.
	6	Resume normal operations, then document the cause.

Prevention 
Remote backend with locking, bucket versioning, least-privilege IAM, protected branches and approval gates for applies, prevent_destroy lifecycle rules on critical resources, and regular drift checks in CI/CD.

