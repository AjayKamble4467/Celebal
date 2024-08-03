## Combined Azure CLI Script

#!/bin/bash

# Variables
resourceGroupName="MyResourceGroup"
location="eastus"
vnetName="MyVNet"
subnetName="MySubnet"
bastionSubnetName="AzureBastionSubnet"
vmName1="MyVM1"
vmName2="MyVM2"
adminUsername="azureuser"
adminPassword="Password123!"
publicIPName="MyPublicIP"
lbName="MyLoadBalancer"
frontendIPConfigName="MyFrontEndConfig"
backendPoolName="MyBackendPool"
probeName="MyHealthProbe"
lbRuleName="MyLBRule"
bastionPublicIPName="MyBastionPublicIP"
bastionName="MyBastion"
natGatewayPublicIPName="MyNatGatewayPublicIP"
natGatewayName="MyNatGateway"

# Create Resource Group
az group create --name $resourceGroupName --location $location

# Create Virtual Network and Subnets
az network vnet create --resource-group $resourceGroupName --name $vnetName --address-prefix 10.0.0.0/16 --subnet-name $subnetName --subnet-prefix 10.0.1.0/24

# Create Subnet for Azure Bastion
az network vnet subnet create --resource-group $resourceGroupName --vnet-name $vnetName --name $bastionSubnetName --address-prefix 10.0.2.0/24

# Create Virtual Machines
az vm create --resource-group $resourceGroupName --name $vmName1 --image UbuntuLTS --admin-username $adminUsername --admin-password $adminPassword --vnet-name $vnetName --subnet $subnetName --nics ${vmName1}Nic

az vm create --resource-group $resourceGroupName --name $vmName2 --image UbuntuLTS --admin-username $adminUsername --admin-password $adminPassword --vnet-name $vnetName --subnet $subnetName --nics ${vmName2}Nic

# Create Public IP for Load Balancer
az network public-ip create --resource-group $resourceGroupName --name $publicIPName --sku Standard

# Create Load Balancer
az network lb create --resource-group $resourceGroupName --name $lbName --sku Standard --frontend-ip-name $frontendIPConfigName --public-ip-address $publicIPName

# Create Backend Pool
az network lb address-pool create --resource-group $resourceGroupName --lb-name $lbName --name $backendPoolName

# Create Health Probe
az network lb probe create --resource-group $resourceGroupName --lb-name $lbName --name $probeName --protocol tcp --port 80

# Create Load Balancer Rule
az network lb rule create --resource-group $resourceGroupName --lb-name $lbName --name $lbRuleName --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name $frontendIPConfigName --backend-pool-name $backendPoolName --probe-name $probeName

# Create Public IP for Bastion
az network public-ip create --resource-group $resourceGroupName --name $bastionPublicIPName --sku Standard

# Create Azure Bastion
az network bastion create --resource-group $resourceGroupName --name $bastionName --public-ip-address $bastionPublicIPName --vnet-name $vnetName --subnet $bastionSubnetName

# Create Public IP for NAT Gateway
az network public-ip create --resource-group $resourceGroupName --name $natGatewayPublicIPName --sku Standard

# Create NAT Gateway
az network nat gateway create --resource-group $resourceGroupName --name $natGatewayName --public-ip-addresses $natGatewayPublicIPName --idle-timeout 10

# Associate NAT Gateway with Subnet
az network vnet subnet update --resource-group $resourceGroupName --vnet-name $vnetName --name $subnetName --nat-gateway $natGatewayName

# Add VM1 to Backend Pool
az network nic ip-config address-pool add --resource-group $resourceGroupName --nic-name ${vmName1}VMNic --ip-config-name ipconfig1 --lb-name $lbName --address-pool $backendPoolName

# Add VM2 to Backend Pool
az network nic ip-config address-pool add --resource-group $resourceGroupName --nic-name ${vmName2}VMNic --ip-config-name ipconfig1 --lb-name $lbName --address-pool $backendPoolName

# List all resources
az resource list --resource-group $resourceGroupName




## Running the Script


Save the script: Save the combined script into a file, e.g., setup_azure_lb.sh.
Make the script executable: Run chmod +x setup_azure_lb.sh.
Execute the script: Run ./setup_azure_lb.sh.