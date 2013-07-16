Project Sulfur
==============
Description
-----------
Project Sulfur is the internal name assigned to this task by the customer. The goal of the project is to implement a new architecture that is better scalable, versioned, more secure, and allows team members to evolve it. Part of the project is to migrate existing pieces of the current, ad-hoc, AWS infrastructure to a versioned CloudFormation template and implement a number of suggested improvements.

This directory contains a collection of templates for creating a complete and independent infrastructure inside a VPC.

The project consists of two phases - each with a specific set of deliverables detailed in a separate document.

The finished platform consists of the following elements:
* The whole stack is distributed between two AZ
* All resources are deployed inside a Virtual Private Cloud (VPC) for isolation
* The VPC has two types of subnets - public and private. Access to and between each subnet is defined using Security Groups and Network ACLs. Each subnet/EC2 instance has access and accepts connections only to defined ports and resources. Each class of hosts sits inside a dedicated subnet.
* Two sets of Elastic Load Balancers - one, public, distributing the load to the web and app front-ends and another, internal/private one for the DB back-end connections from the application servers.
* TLS/SSL connections are terminated on the web servers
* All EC2 instance types are tunable
* There is a dedicated EC2 instance to terminate VPN (OpenVPN) and SSH connections and provide egress and ingress access to the servers inside the VPC
* Route 53 hostnames that are automatically populated by the template for the particular environment using a parametrized domain
* IAM identities for the servers and ACLs for accessing to sensitive information (eg. inside private S3 buckets)
* Some EC2 instances are assigned static Elastic IP addresses
* Autoscaling of EC2 combined with CloudWatch
* Each major stack initialization step is signalled using SNS
* The scripts for provisioning the EC2 instances are directly included in each EC2 resource definition in order to provide some (although reduced) level of portability between the different public cloud providers (eg. AWS, Rackspace)
* Database instances use dedicated RAID10 volumes assembled during EC2 initialization

Phase 1
-------
A rough PoC template covering the base suggested architecture is implemented

Phase 2
-------
Individual components of the architecture are separated in their own templates. This reduces the size of each template, provides a defined set of parameters to change the template's initialization and makes further development of the stack easier. The "interface" to each template makes each building block more flexible and the whole architecture could be easily augmented by modifying and including only a subset of the existing templates.
