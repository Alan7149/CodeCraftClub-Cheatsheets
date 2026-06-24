# 🏗️ Terraform Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Terraform (Infrastructure as Code) quick reference.

---

## Core Workflow

```bash
terraform init        # download providers, set up backend
terraform fmt         # format .tf files
terraform validate    # check syntax
terraform plan        # preview changes (dry run)
terraform apply       # create/update infrastructure
terraform destroy     # tear everything down
terraform apply -auto-approve   # skip confirmation prompt
```

## Provider & Resource

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  tags = {
    Name = "web-server"
  }
}
```

## Variables

```hcl
# variables.tf
variable "region" {
  type    = string
  default = "us-east-1"
}

variable "instance_count" {
  type    = number
  default = 2
}

variable "tags" {
  type = map(string)
  default = { Env = "dev" }
}

# usage
provider "aws" { region = var.region }
```

```bash
# pass values
terraform apply -var="region=eu-west-1"
terraform apply -var-file="prod.tfvars"
```

```hcl
# terraform.tfvars
region         = "us-east-1"
instance_count = 3
```

## Outputs

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}

output "instance_id" {
  value       = aws_instance.web.id
  description = "ID of the EC2 instance"
  sensitive   = false
}
```

```bash
terraform output                  # show all
terraform output instance_ip      # one value
```

## References & Interpolation

```hcl
resource "aws_eip" "ip" {
  instance = aws_instance.web.id        # reference another resource
}

# Expression syntax
name = "app-${var.env}-${count.index}"
```

## Meta-Arguments

```hcl
# count
resource "aws_instance" "web" {
  count         = 3
  instance_type = "t2.micro"
  tags = { Name = "web-${count.index}" }
}

# for_each
resource "aws_instance" "app" {
  for_each      = toset(["a", "b", "c"])
  instance_type = "t2.micro"
  tags = { Name = "app-${each.key}" }
}

# depends_on
resource "aws_instance" "db" {
  depends_on = [aws_security_group.db]
}
```

## State Management

```bash
terraform state list              # list resources in state
terraform state show aws_instance.web
terraform state rm aws_instance.web    # remove from state (not infra)
terraform import aws_instance.web i-12345  # bring existing into state
terraform refresh                 # sync state with real infra
```

## Remote Backend (team state)

```hcl
terraform {
  backend "s3" {
    bucket = "my-tf-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}
```

## Modules (reuse)

```hcl
module "vpc" {
  source  = "./modules/vpc"
  cidr    = "10.0.0.0/16"
}

# use module output
resource "aws_instance" "web" {
  subnet_id = module.vpc.subnet_id
}
```

## Tips

```hcl
# Data source: read existing infra
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]
}

# Conditional
instance_type = var.env == "prod" ? "t3.large" : "t2.micro"
```

> Keep `.tfstate` out of git and **never** commit secrets — use a remote backend + variables.

---

[🔝 Back to README](../README.md)
