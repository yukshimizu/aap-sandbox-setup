# AWS

## Environment to be set up
- Single VPC
- Single subnet
- Single route table
- Single gateway
- Single Ansible Automation Platform

## Playbooks
|Name     |Role Used|Description|
|:--------|:--------|:----------|
|`create_networks.yml`|N/A|Create required AWS network resources.|
|`delete_networks.yml`|N/A|Delete AWS network resources created in `create_networks` playbook.|
|`create_aap_vm.yml`|[roles.aap](../roles/aap/README.md)|Create an AWS instance and set up Ansible Automation Platform.|
|`delete_aap_vm.yml`|N/A|Delete the instance created in `create_aap_vm` playbook.|


## Prerequisites
### Basic requirements for Ansible
Any control node:
- ansible core 2.18+

Ansible collections:
- amazon.aws
- redhat.rhel_system_roles

In order for functioning Ansible EC2, you need to install Python Boto3 library.
```
# pip3 install boto3
```
### ansible.cfg
Update the following line with your EC2 private key file.
```
[default]
private_key_file = "path to EC2 private key file"
```
### Environment variables
In this setup, you should set the following environment variables on your control node.
```
$ export AWS_DEFAULT_REGION=ap-northeast-1
$ export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
$ export AWS_SECRET_ACCESS_KEY=wJatrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```


## Usage
### Create required network resources
These variables should be set in group_vars beforehand.
```
aws_vpc: demo_vpc
aws_vpc_cidr_block: 10.0.0.0/16 # adjust with your preference
aws_vpc_subnet_name: demo_subnet
aws_subnet_cidr_block: 10.0.1.0/24 # adjust with your preference
aws_igw_name: demo_gtw
aws_routetable_name: demo_rtb
aws_securitygroup_name: demo_sg

purpose: demo
```

This playbook need to be run at the beginning.
```
$ cd aws
$ ansible-playbook create_networks.yml
```

### Create Ansible Automation Platform
These variables should be set in group_vars beforehand.
```
aws_vpc_subnet_name: rhel_demo_subnet
aws_securitygroup_name: rhel_demo_sg
aws_aap_instance_ami: ami-07b1660a28c21d579 # ami of RHEL-9.6.0_HVM-20251030-x86_64-0-Access2-GP3
aws_aap_instance_size: t2.xlarge # should not be modified

aap_vm_type: aap # should not be modified
aap_vm_name: aap01

purpose: demo
```

And, the following variables are prompted at run-time. Also refer to [roles.aap](../roles/aap/README.md) for the role details.
```
aws_keypair_name # Your AWS key pair name corresponding to the private key
rhsm_username # Your Red Hat login name
rhsm_passwd # Password for your Red Hat login
aap_admin_passwd # Password for your AAP admin user
aap_pg_passwd # PostgreSQL password for your AAP deployment
```

This playbook can run after running `create_networks` playbook.
```
$ cd aws
$ ansible-playbook create_aap_vm.yml
```

### Clean up the environment
All the delete resource playbooks corresponding to each create resource playbook are avaialble. Those playbooks can run assuming related variables have already set previously.
