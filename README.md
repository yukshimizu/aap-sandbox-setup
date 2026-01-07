# aap-sandbox-setup
This repo includes ansible playbooks and roles for setting up a sandbox environment of [Ansible Automation Platform (AAP)](https://www.redhat.com/en/technologies/management/ansible) on public cloud platform. The sandbox AAP is deployed as [container growth topology](https://docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/2.6/html-single/tested_deployment_models/index#cont-a-env-a) in a single VM for demo purpose.

## Project structure
|Directory           |Description|
|:-------------------|:----------|
|[aws](aws/README.md)|Playbooks for deployng on AWS.|
|azure (future)      |Playbooks for deployng on Azure.|
|roles               |Shared roles used by above playbooks.|

### Roles
|Name                |Description|
|:-------------------|:----------|
|[aap](roles/aap/README.md)|A role to create an Ansible Automation Platform.|


## Required subscriptions
Basically, the demo environment requires to get access to Red Hat gold image, so you must have a matching Red Hat product subscription and must connect their cloud provider accounts to Red Hat.
Please refer to the [Cloud Access user interface](https://access.redhat.com/management/cloud) or Cloud Sources on [cloud.redhat.com](https://cloud.redhat.com/) as described in [Chapter 8, Red Hat Cloud Access program overview](https://access.redhat.com/documentation/en-us/subscription_central/2023/html/red_hat_cloud_access_reference_guide/getting-started-with-ca_cloud-access).

Required subscriptions:
- Red Hat Enterprise Linux for x86_64
- Red Hat Ansible Automation Platform Subscription