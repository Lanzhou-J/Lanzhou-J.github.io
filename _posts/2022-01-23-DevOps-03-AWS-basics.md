---
title: DevOps 03 - AWS basics
layout: post
subtitle: null
date: '2022-1-23 22:39:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- study
- English
- wiki
- devops
---
**Research summary:**

- VPCs
- Routing Table
- Gateway
- Security Groups / NACLs
- DNS

**Why we need to learn these concepts/services?**
DevOps need to deploy and manage AWS resources in a networked environment that provides workload isolation.

# 1 VPC
## 1.1 VPC

### What is VPC
Firstly about VPC - virtual private cloud

- VPC is our private Network space in AWS Cloud.
- it provides logical isolation for our workloads (e.g. Dev/Test/Production)
- It also allows custom access controls and security settings for your resources. (Provide strict access rules for inbound and outbound traffic.)
- **Amazon VPC provides your resources with a virtual private network defined by a set of available IPs called a CIDR range.** (Definition of Amazon VPC)

When you create a VPC, you must specify an IPv4 CIDR block for the VPC. The allowed block size is between a /16 netmask (65,536 IP addresses) and /28 netmask (16 IP addresses).

- A little trick of mine is to use 10.0.0.0/8 for production VPCs, 172.16.0.0/12 for Test, and 192.168.0.0/16 for development. That helps me remember what environment I'm dealing with.

### Deploying a VPC

- VPC deploy into 1 of the 18 AWS Regions (First step, choose 1 region)
- A VPC can host resources from any Availability Zone within its region
- Can have up to 5 VPCs per Region per account by default

## 1.2 Subnets

### Using subnets to divide your VPC
A subnet is a segment or partition of a VPC's IP address range where you can isolate a group of resources. (e.g. public subnet, private subnet — just names!!!)

### Subnets: key attributes

- Subnets are a subset of the VPC CIDR block
- Subnet CIDR blocks cannot overlap
- Each subnet resides entirely within one Availability Zone
- An Availability Zone can contain multiple subnets
- From each subnet AWS reserves 5 IP addresses - router needs them. (used for routing, Domain Name System (DNS), and network management.)


# 2 Routing table
### Route Tables: Directing Traffic Between VPC Resources

Route Tables:

- Required to direct traffic between VPC resources
- Each VPC has a main route table (default)
- You can create custom route tables
- All subnets must have an associated route table

Best practice: Use custom route tables for each subnet.

### Subnets Allow Different Levels of Network Isolation

Public subnets:

- Include a routing table entry to an internet gateway to support inbound/outbound access to the public internet.

Private subnets:

- Do not have a routing table entry to an internet gateway.
- Are not directly accessible from the public internet
- Typically use a NAT gateway to support restricted, outbound public internet access.

# 3 Gateways

### Connecting Public Subnets to the Internet

Internet Gateways

- Allow communication between instances in your VPC and the internet
- Are horizontally scaled, redundant and highly available by default
- Provide a target in your subnet route tables for internet-routable traffic

### Connecting Private Subnets to the Internet

NAT Gateways (network address translation (NAT) gateway)

- Enable instances in the private subnet to initiate outbound traffic to the internet or other AWS services.
- Prevent private instances from receiving inbound traffic from the internet.
- Just like（Heart Valve）— One direction.


# 4 Security Groups/NACLs

## 4.1 Security Groups

- Virtual firewalls that control inbound and outbound traffic into AWS resources
- Traffic can be restricted by any IP protocol, port or IP address
- Rules are stateful
    - If allow in, allow out (Security groups are stateful—if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of the inbound rules.)
    - meaning request information is tracked, and so responses won't need to be tracked as a new request. Ex: ICMP ping requests.
- By default, block all inbound traffic but allow all outbound traffic

## 4.2 NACLs

- Firewalls Acts at the subnet boundary
- Will allow all inbound and outbound traffic by default
- Are stateless , requiring explicit rules for both inbound and outbound traffic.
    - Just like passport, allow in dose not means allow out at the same time.

NACL — Subnet level — Explicitly deny

# 5 DNS

# 5.1 Route 53
Speaking of DNS - Domain Name System service, it's time to discuss about Route 53

Route 53 is a highly available and scalable cloud Domain Name System (DNS) service.

- DNS translates domain names into IP addresses
- Able to purchase and manage domain names and automatically configure DNS settings
- Provides tools for flexible, high-performance, highly available architectures on AWS.
- Multiple routing options.

### How Does Route 53 Help with High Availability?

Route53 → fail to connect to ELB in Oregon region → Route53 provides DNS health checks → Allow for DNS Failover → connect to ELB in London Region.

### Route 53 Routing Options

- Simple round robin

- Weighted round robin (A/B testing)

- Latency-based routing

- Health check and DNS failover

- Geolocation routing

- Geoproximity routing with traffic biasing

Useful Resources:
