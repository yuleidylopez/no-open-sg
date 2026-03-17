![Terraform](https://img.shields.io/badge/Terraform-IaC-blue)
![Policy as Code](https://img.shields.io/badge/Policy--as--Code-Rego-green)
![Security](https://img.shields.io/badge/Security-Validated-red)

# 🚫 Block Open Security Groups with Policy as Code

## Project Overview
This project demonstrates how to detect and prevent insecure cloud configurations using **Policy as Code**.

The policy blocks security group rules that allow public internet access (`0.0.0.0/0`) to sensitive ports such as:

- SSH (Port 22)
- RDP (Port 3389)

These misconfigurations are among the most common causes of cloud breaches.

---

## Security Problem

Many cloud deployments accidentally expose servers to the public internet.

Example of a dangerous configuration:

0.0.0.0/0 → Port 22 (SSH)

Attackers continuously scan for these open ports to gain unauthorized access.

---

## Solution

This project uses:

- Terraform to simulate infrastructure
- Rego policy to define security rules
- Conftest to evaluate configurations

If Terraform attempts to deploy a resource with open SSH or RDP access, the policy **fails the test and blocks deployment**.

---

## Tools Used

- Terraform
- Rego (Open Policy Agent)
- Conftest
- GitHub Codespaces

---

## Project Structure

no-open-sg/

README.md

sg.tf

conftest.toml

policy/

deny-open-sg.rego


---

## How to Run the Test

Inside the Codespace terminal:

Initialize Terraform:

terraform init

Create Terraform plan:

terraform plan -out=tfplan.binary

Convert plan to JSON:

terraform show -json tfplan.binary > input.json

Run policy test:

conftest test input.json --all-namespaces

Expected result:

FAIL - input.json - main - Open SSH access (port 22) to the internet is not allowed.

---

## Fixing the Configuration

Update the insecure CIDR block in `sg.tf`:

Replace:

0.0.0.0/0

With a restricted internal network:

10.0.0.0/16

Re-run the test to verify that the configuration now passes.

---

## Compliance Mapping

This policy supports several security controls from NIST SP 800-53:

AC-4 – Information Flow Enforcement  
AC-6 – Least Privilege  
SC-7 – Boundary Protection  
SC-7(3) – Deny by Default  
SI-4 – System Monitoring

By preventing public exposure of management ports, the policy reduces attack surface and enforces secure network boundaries.

---

## Why This Matters

Open security groups are a leading cause of cloud security incidents.  
Automating security checks with policy-as-code helps organizations:

- Prevent misconfigurations
- Enforce security policies automatically
- Maintain compliance with federal frameworks
- Protect cloud infrastructure from external attacks

---

## Author

Yuleidy Lopez  
Cloud Security / GRC Portfolio Project
