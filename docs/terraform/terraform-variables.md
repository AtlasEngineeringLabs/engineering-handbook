# Terraform Variables

## Why Variables Exist

They make configurations reusable across environments (dev/staging/prod), teams, or projects without duplicating or rewriting .tf code. They also keep sensitive or environment-specific values out of the core logic.

Example: the same main.tf can deploy to dev or prod just by changing the region and project_id values passed in — no code changes.

## Input Variables

Declared in a variable block and referenced via var.<name> elsewhere in the configuration. They're the mechanism for supplying values from outside the config (CLI, files, env vars).

variable "instance_count" {
  type = number
}

resource "google_compute_instance" "vm" {
  count = var.instance_count
  # ...
}

## Variable Declaration

## Variable Types

Terraform enforces a type constraint if you declare one — string, number, bool, list, map, set, object, tuple. Helps catch mistakes early (e.g. passing text where a number is expected).

variable "allowed_ips" {
  type = list(string)
}

variable "tags" {
  type = map(string)
  default = {
    environment = "dev"
  }
}

## Default Values

Makes a variable optional — if no value is supplied, Terraform uses the default. Good for non-critical/safe settings; avoid for security- or environment-sensitive values (as discussed earlier — e.g. project_id, credentials).

variable "machine_type" {
  type    = string
  default = "e2-medium"
}

## Variable Validation

Custom conditions attached to a variable to catch bad input before plan/apply runs, rather than failing later at the provider/API level.

variable "environment" {
  type = string

  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

## Local Values

Internal computed values, not set from outside the config. Useful for reducing repetition or combining variables into a derived value.

locals {
  name_prefix = "myapp-${var.environment}"
}

resource "google_storage_bucket" "data" {
  name = "${local.name_prefix}-bucket"
}

## Variable Files (.tfvars)

Files holding variable values, kept separate from .tf logic — typically one per environment.

# prod.tfvars
region       = "us-east1"
project_id   = "my-prod-project"
machine_type = "e2-standard-4"

terraform apply -var-file=prod.tfvars

## Environment Variables (TF_VAR_*)

Terraform automatically reads any environment variable prefixed with TF_VAR_ as a variable value — useful in CI/CD pipelines or for injecting secrets without writing them to disk.

export TF_VAR_project_id="my-prod-project"
terraform apply


## Variable Precedence

When a variable is set in multiple places, Terraform resolves it in this order (highest wins):

1) -var or -var-file flags on the CLI (later flags override earlier ones)
2) *.auto.tfvars files (alphabetical order)
3) terraform.tfvars file
4) TF_VAR_* environment variables
5) Variable default value (lowest priority)

Example: if TF_VAR_region=us-west1 is set but you also run terraform apply -var="region=us-east1", the CLI flag wins — us-east1 is used.

## Best Practices

Declare explicit types and descriptions for every variable; avoid defaults on security- or environment-critical values (project IDs, credentials, network ranges); use validation blocks to catch bad input early; keep secrets out of .tfvars files committed to version control (use TF_VAR_* or a secrets manager instead); use separate .tfvars files per environment; and use locals to derive values rather than repeating expressions across resources.

## Common Mistakes

❌ Hardcoding production values
❌ Omitting variable types
❌ Overusing variables for values that never change
❌ Using variables where locals are more appropriate

## Key Takeaways

1. Variables make Terraform reusable.
2. Variables separate infrastructure logic from configuration values.
3. variables.tf is the conventional place to declare variables.
4. .tfvars files allow different environments to use the same Terraform code.
5. locals are for calculated internal values, not external input.
6. Validation helps prevent invalid configurations before deployment.
