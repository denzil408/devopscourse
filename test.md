# 🚀 Terraform CI/CD with GitHub Actions and Azure

This repository demonstrates how to deploy infrastructure to **Azure** using **Terraform**, automated by **GitHub Actions** with **OIDC authentication** (no secrets needed).

---

## 📦 Workflow Overview

```mermaid
flowchart TD
    A[Developer push / PR / tag] --> B[GitHub Actions: CI]
    B --> C[Terraform fmt & validate]
    C --> D[terraform init<br/>backend = azurerm]
    D --> E[terraform plan -out planfile]
    E --> F[Upload plan artifact<br/>+ PR comment with summary]
    F --> G{Environment gate?}
    G -- "Dev/Staging" --> H[CD: terraform apply]
    G -- "Prod (approval required)" --> I[Manual approval → CD: apply]
    H --> J[Azure Resources]
    I --> J
    D --> S[(Azure Storage Account<br/>Remote State)]
    S -.-> KV[(Azure Key Vault)]



