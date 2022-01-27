---
title: DevOps learning journey 02 - Networking
layout: post
subtitle: null
date: '2022-1-23 12:30:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- study
- English
- wiki
---
**Research summary:**

- IPs and CIDR notation
- DNS
- HTTP
- OSI

# 1 IPs & CIDR notation
## 1.1 IPs

### What is an IP address?
  - Data packets are similar to packages
  - IP address is similar to home address or street number
  - Each device connected to the internet has a unique "ID" -- an IP address. IP addresses allow devices to communicate with each other.

### What is a public IP address?
  - public IP address (external IP address): IP address that can be accessed directly over the internet, assigned to your network router by ISP(internet service provider)

### What is a private IP address?
  - private IP address (local/internal IP address): The address your network router assigns to your device. Each device within the same network is assigned a unique private IP address.

### Differences between private and public IP addresses

### IPv4

- Special IP addresses:
  - local machine IP: 127.0.0.1
  - Special IP Address Ranges:
    - 10.0.0/8 (10.0.0.0 - 10.255.255.255)
    - 172.16.0.0/12 (172.16.0.0 - 172.31.255.255)
    - 192.168.0.0/16 (192.168.0.0 - 192.168.255.255)

### IPv6


## 1.2 CIDR notation

### What is CIDR notation?

- CIDR: Classless Inter-Domain Routing
  - Simplifies routing tables
  - Reduces IPv4 exhaustion

# 2 DNS

## 2.1 How DNS works?

## 2.2 How to set up DNS records

## 2.3 Types of DNS Records

# 3 HTTP

## 3.1 HTTP basic concepts

### What is HTTP?

HTTP is Hyper Text Transfer Protocol

## 3.2 GET and POST

## 3.3 HTTP characteristics


# 4 OSI

## 4.1 OSI model

- OSI model: theoretical stack of 7 layers that can be used to describe the functions of a networking system.

1. Physical layer: carry data through physical hardware. Cable, network interface cards, hubs
2. Data link layer:  Source and destination MAC addresses, Switches
3. Network layer: handles IP addresses, routing, source and destination IP addresses are added.
4. Transport layer: TCP, UDP, Port numbers. Adds the source and destination port numbers.
5. Session layer: Start & Stop Sessions, responsible for establishing and terminating connections between devices.
6. Presentation layer: Formats the data in a way the receiving application can understand. Also able to encrypt or decrypt data if needed.
7. Application layer: Application and user are communicated. SMTP(Simple Mail Transfer Protocol), FTP, Telnet.

(Add OSI model graph or table)

APSTNDP -> "Australia Post Still They Never Deliver Package"

Useful Resources:

[[Video] IPv4, CIDR and VPC Subnets Made Simple](https://www.youtube.com/watch?v=z07HTSzzp3o&t=74s)

[Public vs. Private IP Addresses: What’s the Difference?](https://www.avast.com/c-ip-address-public-vs-private#gref)

[DNS explained](https://www.better.dev/dns-explained-how-your-browser-finds-websites)
