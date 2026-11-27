## Diagram 1 — Module Composition

```mermaid
flowchart TD
    Root["Root Module<br/>Infrastructure Orchestration"]

    Root --> Network["Network Module<br/>VPC and Subnet"]
    Root --> Compute["Compute Module<br/>VM and Boot Disk"]

    Network --> Output["Output: subnet_id"]
    Output --> Input["Input: subnet_id"]
    Input --> Compute

    Network -.-> State[("Shared Root State")]
    Compute -.-> State

    style Root fill:#334155,color:#fff
    style Network fill:#0f766e,color:#fff
    style Compute fill:#1d4ed8,color:#fff
    style State fill:#475569,color:#fff
```

### Key Learning Points

- The root module orchestrates infrastructure components.
- The network module exposes the subnet ID.
- The compute module consumes the subnet ID.
- Output-to-input references create implicit dependencies.
- Both child modules share the root module's Terraform state.

## Diagram 2 — Reusable Modules and State Isolation

```mermaid
flowchart TD
    Shared["Shared Module Repository<br/>Network and Compute"]

    Shared --> DevRoot
    Shared --> ProdRoot

    subgraph DEV["Development Environment"]
        DevRoot["Development Root<br/>Module v1.3.0"]
        DevInfra["Development Infrastructure"]
        DevState[("Development State")]

        DevRoot --> DevInfra
        DevRoot -.-> DevState
    end

    subgraph PROD["Production Environment"]
        ProdRoot["Production Root<br/>Module v1.0.0"]
        ProdInfra["Production Infrastructure"]
        ProdState[("Production State")]

        ProdRoot --> ProdInfra
        ProdRoot -.-> ProdState
    end

    style Shared fill:#334155,color:#fff
    style DevRoot fill:#0f766e,color:#fff
    style ProdRoot fill:#1d4ed8,color:#fff
    style DevState fill:#475569,color:#fff
    style ProdState fill:#475569,color:#fff
```

### Key Learning Points

- Multiple environments can consume the same module code.
- Each environmentas an independent root configuration.
- Each environment uses a distinct state location.
- Environments can consume different module versions.
- Development changes do not automatically propagate to Production.
- State isolation helps reduce deployment blast radius.

## Diagram 3 — Safe Module Release Promotion

```mermaid
flowchart TD
    A["Module Development<br/>Code Changes and Testing"]
    B["Versioned Release<br/>Release Notes and Compatibility"]
    C["Development and Test<br/>Initial Deployment Verification"]
    D["UAT<br/>Integration and Acceptance Testing"]
    E["Production Review<br/>Plan and Impact Assessment"]
    F{"Change Approved?"}
    G["Production Deployment"]
    H["Post-deployment Verification"]
    I["Investigate and Remediate"]

    A --> B --> C --> D --> E --> F
    F -->|Yes| G --> H
    F -->|No| I
    I --> A

    style A fill:#334155,color:#fff
    style B fill:#334155,color:#fff
    style E fill:#b45309,color:#fff
    style F fill:#b45309,color:#fff
    style G fill:#15803d,color:#fff
    style H fill:#15803d,color:#fff
    style I fill:#b91c1c,color:#fff
```

### Key Learning Points

- Module releases should be versioned.
- Changes should be tested in lower environments.
- Production requires an environment-specific Terraform plan.
- tomated tests do not replace engineering approval.
- Unacceptable changes must be investigated before deployment.
- Post-deployment verification is part of the change lifecycle.
