# aap-sandbox-setup
This repo includes ansible playbooks and roles for setting up a sandbox environment of [Ansible Automation Platform (AAP)](https://www.redhat.com/en/technologies/management/ansible) on public cloud platform. The sandbox AAP is deployed as [container growth topology](https://docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/2.6/html-single/tested_deployment_models/index#cont-a-env-a) in a single VM for demo purpose.

## Project structure
|Directory           |Description|
|:-------------------|:----------|
|[aws](aws/README.md)|Playbooks for deployng on AWS.|
|[azure](azure/README.md)|Playbooks for deployng on Azure.|
|roles               |Shared roles used by above playbooks.|

### Roles
|Name                |Description|
|:-------------------|:----------|
|[aap](roles/aap/README.md)|A role to create an Ansible Automation Platform.|


## Required subscriptions
Basically, the demo environment requires to get access to Red Hat gold image, so you must have a matching Red Hat product subscription and must connect their cloud provider accounts to Red Hat.
Please refer to the [Cloud Access user interface](https://access.redhat.com/management/cloud) or Cloud Sources on [cloud.redhat.com](https://cloud.redhat.com/) as described in [Red Hat Cloud Access program overview](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_rhel_system_registration/con-red-hat-cloud-access-program-overview).

- AWS - [Using gold images on Amazon Web Services (AWS)](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_rhel_system_registration/con-red-hat-cloud-access-program-overview#assembly-using-gold-images-on-aws_system-registration-rhel)
- Azure - [Using gold images on Azure](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_rhel_system_registration/con-red-hat-cloud-access-program-overview#assembly-using-gold-images-on-azure_system-registration-rhel)

Required subscriptions:
- Red Hat Enterprise Linux for x86_64
- Red Hat Ansible Automation Platform Subscription