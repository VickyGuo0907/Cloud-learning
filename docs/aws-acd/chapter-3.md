# Chapter 3. Compute Services in AWS

## Quiz

1. VPC A is peered to VPC B. VPC B is peered to VPC C. You have set up routing in VPC A, which lists the VPC C subnet as a subnet of VPC B. You are trying 
to ping an instance in VPC C from VPC A, but you are not getting a response. Why?
    * Transient connections are not supported by VPC peering.
    * Your security groups of the VPC C instance do not allow incoming pings.
    * You need to create an NACL in VPC C that will allow pings from VPC A.
    * The NAT service in VPC B is not configured correctly.

2. You are tasked with migrating an EC2 instance from one availability zone to another. Which approach would be the best to achieve full data consistency?
    * Shut down the instance. Then restart the instance and select the new availability zone.
    * Keep the instance running. Select Migrate to AZ in the instance actions and select the new availability zone.
    * Shut down the instance, create a snapshot, start a new instance from the snapshot, and select the new availability zone.
    * Keep the instance running. Create a snapshot with the no-shutdown option. Start a new instance from the snapshot and select the new availability zone.

3. You are required to select a storage location for your MySQL database server on an EC2 instance. What AWS service would be the most appropriate for such an object?
    * RDS
    * EBS
    * EFS
    * S3

4. With ECS, what allows you to control high availability of a containerized application?

    * Placement of ECS tasks across ECS instances
    * Placement of ECS tasks into an ECS cluster
    * Placement of ECS instances across regions
    * Placement of ECS instances across availability zones

5. Which scripting languages are supported in a CloudFormation template? (Choose two.)
    * YAML
    * Ruby DSL 
    * JSON
    * Python

6. To set up a route from an on-premises location to a VPC subnet through Direct Connect, which of the following do you need to use?
    * RIPv2
    * RIPv1
    * Static routing
    * BGP

7. To change the number of instances in an Auto Scaling group from 1 to 3, which count do you set to 3?

    * Percentage
    * Maximum instances
    * Desired instances
    * Running instances

8. To maximize IOPS in an EBS volume, which of the following would you need to select?
    * Provisioned IOPS volume
    * General purpose volume
    * Disk-backed volume
    * Dedicated IOPS volume

9. To automate the infrastructure deployment of a three-tier application, which of the following options could you use? (Choose all that apply.)
    * CloudFormation
    * CLI
    * CloudTrail
    * OpsWorks Stacks

10. Which of the following compute options would be best suited for a tiny 100 MB microservices platform that needs to run in response to a user action?
    * Lambda
    * EC2
    * ECS
    * EKS

## Foundation Topics

### Notes
* Modern networking requirements are typically divided into two categories:
    * **Local area network (LANs)**: These are private networks that allow communication only within a certain limited set of network addresses 
    (usually) within one organization.
    * **Wide area network (WANs)**: These are either private or public networks that are designed to allow communication at a distance with multiple parties. When these 
    networks are publc, the term WAN is usually replaced with the Internet
* Internet Protocal comes in two versions: version 4 (IPv4) and version 6 (IPv6)
    * The IPv4 protocol has a 32-bit addressing field.
    * The IPv6 protocol has a 128-bit addressing field.

## Networking in AWS

The following are the most important networking tools available in AWS

* **Amazon Virtual Private Cloud (VPC)**: A service for creating logically isolated networks in the cloud
* **VPC network ACLs and security groups**: Tools for securing network and instance access in VPC
* **AWS Direct Connect and VPC gateways**: Tools for connecting your on-premises networks with AWS
* **Amazon Route 53**: A next-generation DNS service with an innovative API that allow for programmatic access to the DNS services
* **Amazon CloudFront**: A dynamic caching and CDN service in the AWS cloud
* **Amazon Elastic Load Balancing(ELB)**: Load balancing as a service in the AWS cloud
* **Amazon Web Application Firewall (WAF)**: A tool that protects web applications from external attacks using exploits and security vulnerabilities
* **AWS Shield**: An AWS managed DDoS service

### Amazon Virtual Private Cloud (VPC)




## Quiz Answer
1. A. Transient connections are not supported by VPC peering.
2. C. Shut down the instance, create a snapshot, start a new instance from the snapshot, and select the new availability zone.
3. B. EBS
4. D. Placement of ECS instances across availability zones
5. A. YAML C. JSON
6. D. BGP
7. C. Desired instances 
8. A. Provisioned IOPS volume
9. A. CloudFormation B. CLI D OpsWorks Stacks
10. A. Lambda
