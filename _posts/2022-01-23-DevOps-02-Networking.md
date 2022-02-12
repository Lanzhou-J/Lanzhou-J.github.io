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


#### IPv4

IPv4(Internet Protocol version 4) address is represented by 32 digit positive integers, IP address is processed in binaries by computers.

IPv4 addresses are most often written in dotted decimal notation, which is easier for humans to read and remember. Each 8-bit byte in the 32-bit IPv4 address is converted from binary format to a decimal number between 0 and 255. The numbers are written as 4 decimal numbers with dots between them (e.g. W.X.Y.Z)

![IPV4 address](/img/in-post/IPv4.png)

- dotted decimal notation: `Octet#1.Octet#2.Octet#3.Octet#4`

So, the max number of IP address is 2^32 = 4294967296. (i.e., around 4.3 billion computers can be connected to the internet) In fact, one host computer, or one router can occupy 1, 2 or more IP addresses, so computers that can actually connect to the internet would be less than 4.3 billion.

Nowadays, not only computers, mobile phones, iPad would need IP addresses too, so we would expect the IP addresses needed are more than 4.3 billion. How to solve this problem? Network Address Translation (NAT) is used to limit the number of public IP addresses an organization or company must use, for both economy and security purposes. With NAT technology, more than 4.3 billion computers/devices can be connected to the internet.

##### Special IP addresses:
  - local machine IP: 127.0.0.1
  - Private Address Ranges:
    - Class A: 10.0.0/8 (10.0.0.0 - 10.255.255.255)
    - Class B: 172.16.0.0/12 (172.16.0.0 - 172.31.255.255)
    - Class C: 192.168.0.0/16 (192.168.0.0 - 192.168.255.255)

##### Classful Networking (1981-1993)
![classful-networking](/img/in-post/classful-networking.png)
A classful network is used from 1981 until the introduction of CIDR(Classless Inter-Domain Routing) in 1993. The method divides IP addresses range into 5 classes based on the leading 4 digits bits. (Class A, B, C, D and E)

| Class | IP address range | Max number of hosts |
| -------- | -------- | -------- |
| A     | 0.0.0.0 ~ 127.255.255.255 | 16777214 |
| B     | 128.0.0.0 ~ 191.255.255.255 | 65534 |
| C     | 192.0.0.0 ~ 223.255.255.255 | 254 |

The number of addresses usable for addressing specific hosts in each network is always 2N - 2, where N is the number of rest field bits, and the subtraction of 2 adjusts for the use of the all-bits-zero host value to represent the network address and the all-bits-one host value for use as a broadcast address.
- Classful Networking drawbacks:

    1. No address layers within the same network, lacking address flexibility.

    2. Class A, B and C are hard to use in real-life:

    - Class C max number of hosts is too small, only 254.
    - Class B max number of hosts is too large, very wasterful because usually companies would not be able to use all 60 thousand addresses.

    How to solve the problems? See `CIDR`.


#### IPv6

IPv6(Internet Protocol version 6) was introduced in late 1990s as a replacement for IPv4. It uses 128-bit addresses formatted as 8 groups of 4 hexadecimal numbers separated by colons (e.g. fe80::250:56ff:fe86:1610). This largely increases the number of IP addresses we can use (3.4*10^38). There is a saying "It is large enough for every grain of sand on earth to be IP addressable." And this also means that every device on the internet can have a unique IPv6 address.

Compared with IPv4, IPv6 is better in security and extensibility. IPv6 is designed for end-to-end encryption, which will make man-in-the-middle attacks more difficult.


### What is a public IP address?
  - public IP address (external IP address): IP address that can be accessed directly over the internet, assigned to your network router by ISP(internet service provider)

### What is a private IP address?
  - private IP address (local/internal IP address): The address your network router assigns to your device. Each device within the same network is assigned a unique private IP address.

### Differences between private and public IP addresses



| Public IP address | Private IP address |
| -------- | -------- |
| External (global) reach     | Internal (local) reach     |
| Used for communicating outside the private network     | Used for communicating within the private network     |
| A unique ID that is not reused    | A non-unique ID that might be reused in other private networks     |
| Not free     | Free     |
| IP range: Any number not included in the reserved private IP address range (e.g. 8.8.8.8)     |  IP range: see special IP address ranges (e.g. 10.11.12.13)    |


- Public & private IP address example:
![NAT](/img/in-post/NAT.png)

  - Public IP address -> apartment building number
  - Private IP address -> home address


## 1.2 CIDR notation

Since the design of classful networking has limitations, so classless routing solution is proposed, also known as `CIDR`.

### What is CIDR notation?

- CIDR: Classless Inter-Domain Routing
  -  CIDR notation specifies an IP address, a slash('/') character, and a decimal number.
  - 32-bit IP address are separated into 2 parts: network prefix and host identifier.

  How to separate network prefix and host identifier?
  - For example, `a.b.c.d/x`, `/x` represents that the previous x digits are network prefix, and the range of x is `0~32`. This will make IP address more flexible.
  - `10.100.122.2/24`: `/24` represents the first 24 digits are network prefix, and the rest 8 digits are host identifier.


| CIDR | 10.100.122.2/24 |
| -------- | -------- |
| Number of available addresses | 254 |
| subnet mask | 255.255.255.0 |
| network prefix | 10.100.122.0 |
| the first available address | 10.100.122.1 |
| the last available address | 10.100.122.254 |
| broadcast address | 10.100.122.255 |

- Benefits of CIDR:
  - CIDR can be used to effectively manage the available IP address space.
  - CIDR can reduce the number of routing table entries.

# 2 DNS

When we browse websites on the internet, normally we use domain names instead of IP addresses because domain names are easier to remember.

DNS(Domain name system) can achieve this, DNS can help "translate" domain names to specific IP addresses.

## 2.1 How DNS works?

- We type in a domain name (`www.example.com`) into our browser.
- The browser checks its cache and the computer's cache for the DNS records for that match the domain name we entered. If it succeeds, it requests the page from the website's host.
- If we haven’t found our record yet, our request goes to our Recursive DNS Servers that we have set for our computer or network (probably our ISP). If they have the record cached, we take the results from them and try to load the page (and we also cache it locally for later use).
- If we still haven’t found it, we go to the Root DNS Servers, and ask them where to find the correct Top Level DNS Servers for the `.com` TLD.
- We arrive at the `.com` Top Level DNS Servers, who have one nugget of information for us - they are kept up to date on which Authoritative Name Servers are responsible for `example.com` and they share that information with us.
- Then, we head over to the Authoritative Name Servers, who give us the record we’re looking for.
- Finally, the result is cached by the recursive DNS servers, and by our local system - and we load our page!



## 2.2 How to set up DNS records

DNS records are a set of information, primarily IP addresses. These records indicate various things about the domain, e.g. where to go when we type in the domain name `example.com` or `www.example.com`, where to go when other subdomains like `employees.example.com` are used, and how to handle email for the domain etc.

We can edit DNS records, usually, on the domain registrar or our website host's control panels. The records themselves live on the above mentioned "Authoritative Name Servers", and are primarily made up of locations (IP addresses and domain names) and time to live numbers.

**TTL(Time to live)**

- DNS records have a “TTL” or time to live setting. This is simply an amount of time that the name servers will allow records to be cached by any of the computers who might store the information about that specific domain and hostname before that cached data must be discarded and reacquired.

## 2.3 Types of DNS Records
- A Records: An A Record is the type of record that tells an incoming request where to find the website they are looking for.

- CNAME Records: A CNAME record (Canonical Name Record) is a type of DNS record that maps an alias name to a true or canonical domain name. You provide another domain name as the value of this record, and the domain name lookup process will simply continue with the new domain name.

- MX Records: MX records help mail requests to a domain find the correct mail transfer agents that are available for it.


# 3 HTTP

## 3.1 HTTP basic concepts

### What is HTTP?

HTTP is Hyper Text Transfer Protocol

In order to understand HTTP, we can separate it into 3 parts:

- HyperText
- Transfer
- Protocol


1. Protocol:

  - 2 or more participants
  - agreement & regulation on behaviors

    We can see HTTP protocols as a protocol used in the computer world, it uses language that computers can understand to set up a regulation for communication between computers (2 or more participants), including related error handling methods (agreement & regulation on behaviors)

2. Transfer
- HTTP is a two-way protocol. (request & response)
- data is transmitted between point A and B but transfer is allowed.
- HTTP is an agreement/regulation for transmitting data between 2 points.

3. Hyptertext
- Hypertext is text beyond normal text, it is a mixture of word, graph and videos, including links.

- HTML is the most commonly used hypertext.


In summary, HTTP is an agreement and regulation for transmitting data (hypertext, including text, graph, music, videos) between 2 points (server & browser or server & server).

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


### Summary

- What happens when you go to example.com?

![summary](/img/in-post/networking-summary.png)

Useful Resources:

[[Video] IPv4, CIDR and VPC Subnets Made Simple](https://www.youtube.com/watch?v=z07HTSzzp3o&t=74s)

[Public vs. Private IP Addresses: What’s the Difference?](https://www.avast.com/c-ip-address-public-vs-private#gref)

[DNS explained](https://www.better.dev/dns-explained-how-your-browser-finds-websites)
