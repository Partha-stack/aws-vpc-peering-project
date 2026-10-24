AWS VPC Peering Project

Overview

This project demonstrates how to establish private network communication between two Amazon Virtual Private Clouds (VPCs) using AWS VPC Peering. It uses Amazon EC2 instances, custom route tables, security groups, and a bastion host to test connectivity and network isolation.

The project also demonstrates how to troubleshoot connectivity by introducing a controlled failure and restoring the required security group rule.

Objectives

Create two separate VPCs with non-overlapping CIDR ranges.

Configure public and private subnets.

Set up an Internet Gateway for public-subnet connectivity.

Establish a VPC peering connection between the two VPCs.

Configure route tables to enable private communication across the peering connection.

Use a bastion host to access private EC2 instances over SSH.

Verify connectivity, network isolation, and controlled failure recovery.

Architecture

The lab contains two VPCs connected through a VPC peering connection.

Component

Configuration

VPC A

ewb-vpc-a — 10.10.0.0/16

VPC A public subnet

ewb-a-public — 10.10.1.0/24

VPC A private subnet

ewb-a-private — 10.10.2.0/24

VPC B

ewb-vpc-b — 10.20.0.0/16

VPC B private subnet

ewb-b-private — 10.20.1.0/24

VPC peering connection

ewb-a-b-peer

Bastion host

10.10.1.10

Private EC2 instance in VPC A

10.10.2.10

Private EC2 instance in VPC B

10.20.1.10

The bastion host provides an SSH entry point to the private instances. The private instances do not require public IP addresses to communicate with each other through VPC peering.

AWS Services Used

Amazon VPC: Isolated virtual networks and subnet configuration.

Amazon EC2: Bastion host and private instances.

VPC Peering: Private connectivity between VPC A and VPC B.

Route Tables: Routing traffic between the VPC CIDR ranges.

Internet Gateway: Internet connectivity for the public subnet.

Security Groups: Controls for SSH access and ICMP connectivity.

SSH: Secure access to the EC2 instances.

Implementation

Created two VPCs with non-overlapping CIDR ranges.

Created public and private subnets and configured route tables.

Attached an Internet Gateway to VPC A and configured the public route.

Launched three EC2 instances: one bastion host and two private instances.

Configured security groups to control SSH and ICMP traffic.

Created and activated the VPC peering connection.

Added routes to the peering connection in the relevant route tables.

Connected to the private instances through the bastion host.

Tested connectivity in both directions between the private instances.

Temporarily removed an ICMP inbound rule to demonstrate a controlled failure, then restored the rule and verified connectivity again.

Terminated the EC2 instances during cleanup.

Connectivity Testing

The lab verified the following scenarios:

Test

Expected or observed result

App-A to App-B ping

Successful after peering and routing configuration

App-B to App-A ping

Successful after peering and routing configuration

Controlled ICMP failure

Ping failed after the relevant inbound ICMP rule was removed

Recovery

Ping succeeded after the ICMP rule was restored

Private instance exposure

Both private instances had no public IP addresses

Cleanup

EC2 instances were terminated

Evidence and Screenshots

The repository includes screenshots documenting the configuration and test results.

VPC and subnet configuration







Internet Gateway and routing







Peering and EC2 instances







Connectivity and failure recovery









Cleanup



Key Learnings

VPC peering enables private communication between VPCs without requiring public IP addresses on the communicating instances.

Peering alone is not sufficient: route tables on both sides must direct traffic to the peering connection.

Security groups must allow the required traffic, and the return path must also be configured.

A bastion host provides controlled SSH access to private instances.

Testing both directions and introducing a controlled failure helps identify routing and security group issues.

Cleaning up lab resources helps avoid unnecessary AWS charges.

Cleanup

The EC2 instances were terminated after testing. Other resources, such as VPCs, subnets, route tables, security groups, the Internet Gateway, and the peering connection, should also be checked and removed if they are no longer required.

Author

Partha Das

Disclaimer

This project was created for educational purposes to practice AWS networking concepts. Resource names and configurations reflect the lab environment.