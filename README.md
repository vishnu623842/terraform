# Terraform CI/CD Automation with GitHub Actions

## Project Overview

This project demonstrates how to integrate **Terraform with GitHub Actions** to automate Infrastructure as Code (IaC) workflows.

The project uses Terraform's Local provider to create and manage a local text file. GitHub Actions automates the Terraform workflow whenever code is pushed to the `master` branch.

The objective is to understand how Terraform and CI/CD pipelines work together to automate infrastructure changes.

## Architecture

```text
Developer
    |
    v
Git Repository
    |
    v
GitHub Repository
    |
    v
GitHub Actions Trigger
    |
    v
Ubuntu Runner
    |
    v
Terraform Init
    |
    v
Terraform Validate
    |
    v
Terraform Plan
    |
    v
Terraform Apply
    |
    v
hello.txt Created / Updated
```

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| HashiCorp Local Provider | Creates and manages local files |
| Git | Version control |
| GitHub | Source code repository |
| GitHub Actions | CI/CD automation |
| Ubuntu | GitHub Actions runner environment |

## Project Structure

```text
terraform-iac-cicd/
│
├── .github/
│   └── workflows/
│       └── terraform.yml
│
├── .gitignore
├── .terraform.lock.hcl
├── main.tf
├── hello.txt
└── README.md
```

> Note: The `.github/workflows/terraform.yml` file is part of the intended workflow structure. The `.terraform/` directory and Terraform state files are excluded from Git.

## Terraform Configuration

The `main.tf` file defines the Terraform configuration.

```hcl
terraform {
  required_providers {
    local = {
      source  = "hashicorp/local"
      version = "~> 2.5"
    }
  }
}

provider "local" {}

resource "local_file" "example" {
  filename = "${path.module}/hello.txt"
  content  = "Hello from Terraform CI/CD!"
}
```

### Configuration Explanation

- **terraform block:** Specifies the required provider.
- **required_providers:** Defines the Local provider and its version constraint.
- **provider block:** Configures the Local provider.
- **resource block:** Defines the file Terraform manages.
- **filename:** Specifies the location of `hello.txt`.
- **content:** Specifies the text written into the file.
- **path.module:** Refers to the current Terraform module directory.

## GitHub Actions CI/CD Pipeline

The workflow is configured in:

`.github/workflows/terraform.yml`

### Workflow Triggers

The pipeline executes when:

1. Code is pushed to the `master` branch.
2. The workflow is manually triggered using `workflow_dispatch`.

### Pipeline Stages

| Stage | Command | Description |
|---|---|---|
| Checkout | `actions/checkout@v4` | Checks out repository code |
| Setup | `hashicorp/setup-terraform@v3` | Installs Terraform |
| Initialize | `terraform init` | Initializes the working directory |
| Validate | `terraform validate` | Validates configuration |
| Plan | `terraform plan` | Previews infrastructure changes |
| Apply | `terraform apply -auto-approve` | Applies the Terraform configuration |

## Running Terraform Locally

Clone the repository:

```bash
git clone https://github.com/vishnu623842/terraform.git
```

Navigate into the project:

```bash
cd terraform
```

Initialize Terraform:

```bash
terraform init
```

Validate the configuration:

```bash
terraform validate
```

Generate an execution plan:

```bash
terraform plan
```

Apply the configuration:

```bash
terraform apply
```

Verify the generated file:

```bash
cat hello.txt
```

Expected output:

```text
Hello from Terraform CI/CD!
```

## GitHub Actions Execution

After pushing changes to the `master` branch:

1. Open the GitHub repository.
2. Navigate to the **Actions** tab.
3. Select **Terraform CI/CD**.
4. Open the latest workflow execution.
5. Review the Terraform automation steps.

Expected execution sequence:

- Checkout Repository
- Setup Terraform
- Terraform Init
- Terraform Validate
- Terraform Plan
- Terraform Apply

## Important Concepts Learned

- Infrastructure as Code (IaC)
- Terraform providers and resources
- Terraform initialization and validation
- Terraform execution plans
- Terraform apply
- Git and GitHub version control
- GitHub Actions workflows
- CI/CD automation
- GitHub-hosted Ubuntu runners
- Workflow triggers and branch configuration
- Terraform state management
- Automated configuration updates

## Important Note

GitHub Actions runs Terraform inside a temporary GitHub-hosted runner.

The `hello.txt` file generated during the GitHub Actions execution is created inside that runner. It is not automatically created on the developer's local machine or committed back to the GitHub repository.

To retain generated files, GitHub Actions artifacts or another suitable storage solution can be used.

Also, this demonstration uses the Local provider rather than provisioning cloud infrastructure.

## Author

**Terraform CI/CD Practical Project**

Developed as part of learning Infrastructure as Code and CI/CD automation using Terraform and GitHub Actions.

---

If you find this project useful, feel free to explore, fork, and improve it.
