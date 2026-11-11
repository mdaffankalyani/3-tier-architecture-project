# Three-Tier Architecture on AWS

A three-tier architecture deployed on AWS that separates the **web**, **application**, and **database** layers inside a custom VPC. The database tier is kept private and is not directly reachable from the internet.

## Architecture Overview

```
                        Internet
                           |
                  [ Internet Gateway ]
                           |
   +-----------------------+------------------------+
   |                     Custom VPC                 |
   |                                                |
   |   Public Subnet(s)                             |
   |   +------------------+   +-----------------+   |
   |   |  Web Tier (EC2)  |   |  Bastion Host   |   |
   |   +--------+---------+   +--------+--------+   |
   |            |                      | SSH        |
   |   Private Subnet(s)               |            |
   |   +--------v---------+             |            |
   |   |  App Tier (EC2)  |<------------+            |
   |   +--------+---------+                          |
   |            |                                    |
   |   +--------v---------+     [ NAT Gateway ]      |
   |   |  Database (RDS)  |     (outbound only)      |
   |   +------------------+                          |
   +------------------------------------------------+
```

## Tiers

| Tier | Service | Purpose |
|------|---------|---------|
| Web | Amazon EC2 | Serves the front end to users |
| Application | Amazon EC2 | Handles business logic and talks to the database |
| Database | Amazon RDS | Stores application data in a private subnet |

## What I Built

- Designed and deployed a three-tier architecture on AWS, separating the web, application, and database layers.
- Hosted the web and application tiers on **EC2** instances.
- Built a custom **VPC** with public and private subnets, using **route tables**, an **internet gateway**, and a **NAT gateway** to control traffic between the tiers and the internet.
- Placed the database tier on **Amazon RDS** in a private subnet, keeping it isolated from direct public access.
- Configured **security groups** to control access between the layers.
- Used a **bastion host** for secure SSH access to resources in the private subnets.

## AWS Services Used

- Amazon VPC (subnets, route tables, internet gateway, NAT gateway)
- Amazon EC2
- Amazon RDS
- Security Groups

## Networking and Security

- **Public subnets** hold internet-facing resources, reached through the internet gateway.
- **Private subnets** hold the application and database tiers, with no direct inbound access from the internet.
- **NAT gateway** lets private instances make outbound requests (updates, packages) without being exposed.
- **Security groups** restrict traffic so each tier only accepts connections from the tier directly before it.
- **Bastion host** is the single controlled entry point for SSH into the private subnets.

## Traffic Flow

1. A user sends a request to the web tier through the internet gateway.
2. The web tier forwards the request to the application tier.
3. The application tier queries the RDS database in the private subnet.
4. The response travels back through the same path to the user.

## Screenshots

Screenshots are stored in the `screenshots/` folder of this repository.

## Key Learnings

- How to design a VPC with public and private subnets and route traffic correctly between them.
- Why isolating the database tier improves security.
- How security groups can enforce least-privilege access between layers.
- How a bastion host and NAT gateway each serve a different purpose in a private network.

## Future Improvements

- Add an Application Load Balancer in front of the web tier.
- Add Auto Scaling for the web and application tiers.
- Enable RDS Multi-AZ for high availability.
- Automate the setup with Terraform or CloudFormation.
