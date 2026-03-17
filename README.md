# Cloud Security Policy: Block Open Security Groups

## Overview
This project demonstrates how to prevent insecure cloud configurations using Policy as Code.

The policy blocks security group rules that allow public access (0.0.0.0/0) to sensitive ports such as SSH (22) and RDP (3389).

## Tools Used
- Terraform
- Rego
- Conftest
- GitHub Codespaces

## The Problem
Many cloud environments accidentally expose servers to the internet.

Example of insecure rule:

0.0.0.0/0 → Port 22 (SSH)

Attackers constantly scan the internet for these open ports.

## The Solution
A Rego policy automatically blocks Terraform configurations that expose sensitive ports.

## How It Works

1. Terraform generates a plan.
2. The plan is converted to JSON.
3. Conftest evaluates the JSON using Rego policies.
4. If a rule violates the policy, deployment fails.

## Run the Test

terraform init

terraform plan -out=tfplan.binary

terraform show -json tfplan.binary > input.json

conftest test input.json --all-namespaces

## Compliance Mapping

This policy supports controls from NIST SP 800-53:

AC-4 – Information Flow Enforcement  
AC-6 – Least Privilege  
SC-7 – Boundary Protection  

## Author
Yuleidy Lopez
