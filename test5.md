sequenceDiagram
    autonumber
    participant Dev as Developer
    participant GH as GitHub
    participant GA as GitHub Actions Runner
    participant Entra as Microsoft Entra ID (Azure AD)
    participant TF as Terraform CLI
    participant Az as Azure

    Dev->>GH: Push / PR / Tag
    GH->>GA: Trigger CI workflow
    GA->>TF: fmt, validate, init, plan
    GA-->>GH: Upload plan artifact + PR comment

    Note over GH,Dev: Approval needed for prod apply

    GH->>GA: Trigger CD workflow
    GA->>Entra: Request OIDC token
    Entra-->>GA: Short-lived Azure access token
    GA->>TF: terraform apply planfile
    TF->>Az: Provision/update resources
    Az-->>TF: Apply complete
    GA-->>GH: Status, release notes

