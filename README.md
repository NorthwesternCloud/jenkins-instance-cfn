# Northwestern Cloudbees Jenkins Instance Template

This repository contains a cloudformation template for creating a stack to run
Cloudbees Jenkins.

It creates two resources, a security group that by default allows inbound SSH
from all Northwestern subnets, and an EC2 instance. The EC2 instance will by
default launch into the subnet created specifically for these jenkins hosts and
will have the EC2 systems manager agent installed and enabled.

The stack has the following parameters:
- `Subnet`: The id of the subnet to launch into. Defaults to the
  `nit-jenkins-PvtAZ1` subnet.
- `VPC`: The id of the vpc to launch into. Defaults to the `nit-shared-svcs`
  vpc.
- `KeyName`: Name of an existing EC2 Keypair within the region where the stack
  is being launched
- `InstanceType`: allowed values are `t2.micro` or `t2.large`. Default is
  `t2.micro`.
- `InstanceAMI`: The AMI to use for the instances. Default is Ubuntu 16.04
  64-bit (`ami-10547475` in us-east-2).
- `InstanceProfile`: The IAM role/instance profile to attach to the instance.
  Defaults to the `jenkins-admin` role in the NIT shared services account.
- `Environment`: for the `Environment` tag. Allowed values are `Test`,
  `Staging`, `Production`. Defaults to `Test`.
- `Owner`: For the `Owner` tag. Defaults to `cloudoperations@northwestern.edu`.

E.g. to launch as a stack using the CLI (note that not all parameters are
supplied below; any not specified will receive the default value):

    aws cloudformation create-stack --stack-name <stack name> --template-body file://jenkins-ec2-instance.yaml --parameters \
    ParameterKey=InstanceType,ParameterValue=<instance type> \
    ParameterKey=KeyName,ParameterValue=<keypair name> \
    ParameterKey=Environment,ParameterValue=<environment>