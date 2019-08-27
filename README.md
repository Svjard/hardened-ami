## Hardened Ubuntu AMI Pipeline

This Packer AMI builder creates a new AMI out of the latest Ubuntu 18.04 AMI, and also provides a cloudformation template that leverages AWS CodePipeline to orchestrate the entire process. It additionally uses AWS inspector to verify the image, provides an easy way to extend the image, and setups the AMI to be used across regions and across accounts in a programmtic way.

## Source code structure

```bash
├── ansible
│   ├── playbook.yaml                       <-- Ansible playbook file
│   ├── requirements.yaml                   <-- Ansible Galaxy requirements to be installed
│   └── roles
│       ├── common                          <-- Upgrades all packages through ``apt``
|       ├── cis                             <-- CIS benchmark requirements to harden the AMI
|       ├── aws                             <-- AWS related items including Cloudwatch Logs Agent, SSM Agent, and AWS Inspector
|       ├── extend                          <-- Extendable role to install additional dependencies
├── buildspec.yml                           <-- CodeBuild spec
├── cloudformation                          <-- Cloudformation to create entire pipeline
│   └── pipeline.yaml
├── packer_cis.json                         <-- Packer template for Pipeline
```

## Cloudformation template

Cloudformation will create the following resources as part of the AMI Builder for Packer:

* ``cloudformation/pipeline.yaml``
    + AWS CodeCommit - Git repository
    + AWS CodeBuild - Downloads Packer and run Packer to build AMI 
    + AWS CodePipeline - Orchestrates pipeline and listen for new commits in CodeCommit
    + Amazon SNS Topic - AMI Builds Notification to HTTPS slack webhook
    + Amazon Cloudwatch Events Rule - Custom Event for AMI Builder that will trigger SNS upon AMI completion

## HOWTO

**Launch the Cloudformation stack**

Region | AMI Builder Launch Template
------------------------------------------------- | ---------------------------------------------------------------------------------
N. Virginia (us-east-1) | [![Launch Stack](images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=AMI-Builder&templateURL=https://s3-eu-west-1.amazonaws.com/TODO/cloudformation/pipeline.yaml)

**Setting Up Your Development Environment**



## Known issues

## Roadmap
