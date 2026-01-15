# Azure

## Environment to be set up
- Single Resource Group
- Single VNet
- Single subnet
- Single network security group
- Single public IP address
- Single network interface
- Single Ansible Automation Platform

## Playbooks
|Name     |Role Used|Description|
|:--------|:--------|:----------|
|`create_networks.yml`|N/A|Create required Azure network resources.|
|`delete_networks.yml`|N/A|Delete Azure network resources created in `create_networks` playbook.|
|`create_aap_vm.yml`|[roles.aap](../roles/aap/README.md)|Create an Azure VM and set up Ansible Automation Platform.|
|`delete_aap_vm.yml`|N/A|Delete the VM created in `create_aap_vm` playbook.|

NOTE: `cloud-init.yml` is used when creating Azure VM for setting up specific configurations for Azure VM.

## Prerequisites
### Basic requirements for Ansible
Any control node:
- ansible core 2.18+

Ansible collections:
- azure.azcollection
- redhat.rhel_system_roles

In order for functioning Azure Ansible, you need to install Python dependencies. Please refer to [azure.azcollection](https://galaxy.ansible.com/ui/repo/published/azure/azcollection/docs/) for more details.

### Azure service principal for Ansible
You need to create an Azure service principal with AzureCLI or Azure PowerShell and authenticate to Azure from Ansible beforehand. Please follow the instruction of [Quickstart: Create an Azure service principal for Ansible](https://learn.microsoft.com/en-us/azure/developer/ansible/create-ansible-service-principal?tabs=azure-cli).

### Environment variables
After creating service principal, you should set the following environment variables on your control node.
```
$ export AZURE_SUBSCRIPTION_ID=<SubscriptionID>
$ export AZURE_CLIENT_ID=<ApplicationId>
$ export AZURE_SECRET=<Password>
$ export AZURE_TENANT=<TenantID>
```

### ansible.cfg
Update the following line with your private key file. Please refer to [Quick steps: Create and use an SSH public-private key pair for Linux VMs in Azure](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys) for more details.
```
[default]
private_key_file = "path to Azure private key file"
```

## Usage
### Create required network resources
These variables should be set in group_vars beforehand.
```
az_location: japaneast # adjust with your preference
az_resource_group: aap_sandbox
az_vnet: demo_vnet
az_vnet_cidr_block: 10.1.0.0/16 # adjust with your preference
az_subnet_cidr_block: 10.1.1.0/24 # adjust with your preference
az_vnet_subnet_name: demo_subnet
az_securitygroup_name: demo_sg

purpose: demo # should not be modified
```

This playbook need to be run at the beginning.
```
$ cd azure
$ ansible-playbook create_networks.yml
```

### Create Ansible Automation Platform
These variables should be set in group_vars beforehand.
```
az_resource_group: aap_sandbox
az_vnet: demo_vnet
az_vnet_subnet_name: demo_subnet
az_securitygroup_name: demo_sg
az_ssh_public_key_path: /path_to/public_key_file # adjust with your preference

purpose: demo # should not be modified

az_aap_vm_image_sku: rhel-lvm97-gen2 # Red Hat Enterprise Linux 9.7 (BYOS) - x64 Gen2
az_aap_vm_size: Standard_D4s_v3 # should not be modified

aap_vm_type: aap # should not be modified
aap_vm_name: aap01
```

And, the following variables are prompted at run-time. Also refer to [roles.aap](../roles/aap/README.md) for the role details.
```
rhsm_username # Your Red Hat login name
rhsm_passwd # Password for your Red Hat login
aap_admin_passwd # Password for your AAP admin user
aap_pg_passwd # PostgreSQL password for your AAP deployment
```

This playbook can run after running `create_networks` playbook.
```
$ cd azure
$ ansible-playbook create_aap_vm.yml
```

### Clean up the environment
All the delete resource playbooks corresponding to each create resource playbook are avaialble. Those playbooks can run assuming related variables have already set previously.
