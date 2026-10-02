# Terraform Outputs

## What are Outputs?
Outputs expose useful information after Terraform has created or read infrastructure. They allow users, other modules, and automation pipelines to consume values such as IP addresses, IDs, and endpoints."

## Why Outputs Exist
Outputs expose values from a configuration so they can be viewed, referenced by other configurations, or consumed by external tools/scripts. Example: exposing a VM's public IP after apply so an engineer doesn't have to dig through the cloud console to find it.

## Output Block Structure
An output block names a value and the expression that produces it, displayed after apply or queried via terraform output. Example:

hcl

output "vm_ip" {
  value = google_compute_instance.vm.network_interface[0].access_config[0].nat_ip
}

## Resource Attributes
Attributes are values given back from Terraform, while Arguments are usually input values you set.

Arguments
↓

Desired State

AND

Attributes
↓

Actual State

## Sensitive Outputs
This is for secreformation like passwords & other sensitive outputs
Examples include:
	•	Database passwords
	•	API keys
	•	Service account credentials
	•	Private keys
	•	Authentication tokens
Marking them as sensitive prevents them from being displayed during terraform apply or i output.

## Outputs in Modules
A module's outputs are how values from inside the module get passed back to the calling configuration, since a module's internal resources aren't directly accessible otherwise. Example: a network module outputs vpc_id, which the root configuration then passes into a compute module.

## Outputs in CI/CD
Pipelines can capture Terraform outputs (via terraform output -json) to feed values into later automation steps, like configuration management or application deployment. Example: a pipeline reads the output "load_balancer_ip" after apply and injects it into a DNS update step.

## Best Practices
Only output values that are actually needed elsewhere, and mark sensitive outputs (e.g. passwords, keys) with sensitive = true so they're hidden from CLI output and logs. Example: output "db_password" { value = random_password.db.result; sensitive = true }.

## Common Mistakes
Outputting sensitive values without the sensitive flag, exposing secrets in logs or CI/CD output. Example: output "api_key" { value = var.api_key } without sensitive = true leaks the key into terraform apply logs and terraform output results.

## Key Takeaways

## Key Takeaways

- Outputs are the mechanism for surfacing values out of a Terraform configuration — whether for a person, a script, or another configuration.
- They're especially necessary for resource attributes, since those values (like IDs or IPs) aren't known until after `apply`.
- In modules, outputs are the *only* way values pass from inside the module back to the caller — nothing inside a module is accessible otherwise.
- `terraform output -json` makes outputs machine-readable, which is what lets CI/CD pipelines chain Terraform into later automation steps.
- Always mark sensitive outputs (passwords, keys, tokens) with `sensitive = true` to prevent them leaking into CLI output, logs, or pipeline artifacts.
- Keep outputs intentional — only expose what's actually needed elsewhere, since every output becomes part of your configuration's "public interface."
