# Azure Load Balancer Setup

## Description

The objective of this project is to use the Azure portal to create a public Azure Load Balancer, configuring it to manage a backend pool containing two virtual machines. Additionally, set up Azure Bastion, a NAT Gateway, a virtual network, and necessary subnets to support the load balancer configuration.

## Business Problem Statement

- Growing e-commerce platform: Experiencing significant upticks in traffic during peak hours, indicating increasing demand.
- Performance issues and downtime: Resulting from the inability to handle surges in traffic effectively, leading to degraded user experience.
- Requirement for robust load balancing: Across multiple servers to evenly distribute incoming requests and maintain high availability and scalability.

## Prerequisites

- An Azure account with an active subscription.

## Steps

1. Clone the repository.
2. Run each of the scripts in order, making sure to update variables as necessary.

## Scripts

1. `create_resource_group.sh` - Creates the resource group.
2. `create_vnet_and_subnets.sh` - Creates the virtual network and subnets.
3. `create_vms.sh` - Creates the virtual machines.
4. `create_lb_and_backend_pool.sh` - Creates the load balancer, backend pool, and load balancer rules.
5. `create_bastion.sh` - Creates Azure Bastion.
6. `create_nat_gateway.sh` - Creates the NAT Gateway.
7. `add_vms_to_backend_pool.sh` - Adds virtual machines to the backend pool.
8. `list_resources.sh` - Lists all resources created.

## Notes

- Ensure you have the Azure CLI installed and configured before running the scripts.
- Update the variable values in each script according to your setup.


