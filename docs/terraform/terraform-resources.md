# Terraform Resources

## What is a Resource?

A block that defines a piece of GCP infrastructure Terraform should create and manage, such as a Compute Engine VM, a VPC network, or a Cloud Storage bucket.

## Resource Block Structure

</> hcl

resource "google_compute_instance" "web" {

  name         = "web-server"

  machine_type = "e2-medium"

}

The resource keyword, followed by the GCP resource type, a local name, and a body of arguments.

## Resource Types

The kind of GCP infrastructure object being managed (e.g. google_compute_instance, google_storage_bucket, google_container_cluster), defined by the google (or google-beta) provider. It tells Terraform which GCP API/resource to manage.

## Resource Names

The local identifier you give a resource (e.g. "web" in google_compute_instance.web), used to reference it elsewhere in the configuration. It's only meaningful within your Terraform code, not the actual GCP resource name/ID.

## Arguments

The inputs you set inside a resource block to configure it (e.g. machine_type, zone, boot_disk). They define the desired state of the GCP resource.

## Attributes

Values exposed by a resource after it's created, either computed by GCP (like self_link or an assigned IP) or set from arguments. Referenced via resource_type.name.attribute.

## Resource References

Using one resource's attribute in another resource or output, e.g. google_compute_network.vpc.self_link used inside a google_compute_instance block. This links GCP resources together and tells Terraform how they relate.

## Implicit Dependencies

Dependencies Terraform automatically detects because one resource references another resource's attribute (e.g. a google_compute_instance referencing a google_compute_network's self_link). No extra configuration needed — Terraform infers the correct order.

## Explicit Dependencies depends_on`)

A manual way to declare a dependency between resources when there's no direct attribute reference to infer it from — e.g. ensuring a google_project_service (API enablement) completes before a dependent resource is created.

## Dependency Graph

The internal graph Terraform builds from resource references and depends_on to determine the correct order for creating, updating, or destroying GCP resources (e.g. enabling an API before creating resource that needs it, or creating a VPC before subnets).

VPC

↓

Subnet

↓

Firewall

↓

Virtual Machine

↓

Load Balancer

## Best Practices

Use implicit dependencies where possible, enable required GCP APIs via google_project_service before dependent resources, keep resource names descriptive and consistent, use variables for project ID/region/zone, and modularize configurations (e.g. separate modules for networking, compute, IAM).

## Common Mistakes

Forgetting to equired GCP APIs before referencing dependent resources, overusing depends_on when implicit references would work, hardcoding project IDs/regions/zones instead of using variables, inconsistent naming across resources, and not reviewing terraform plan before applying.

## Key Takeaways

A resource is the basic unit Terraform manages.
Every resource has a type and a Terraform name.
Arguments define the desired configuration.
Attributes are values Terraform learns after the resource exists.
Terraform builds a dependency graph, not a simple execution order.
Resource references create implicit dependencies, while depends_on creates explicit dependencies when needed.

Terraform Configuration

        │

        ▼

Resources

        │

        ▼

References

        │

        ▼

Dependency Graph

        │

        ▼

Execution Plan

        │

        ▼

Cloud Infrastructure

## Typical interview questions.

## Practical scenarios.

## Common mistakes

## Key terminology

## Revision checklist
