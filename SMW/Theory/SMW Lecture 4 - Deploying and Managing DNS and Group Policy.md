# SMW Lecture 4 - Deploying and Managing DNS and Group Policy

###### tags: #SMW

## Table of Contents
```toc
```

## Implementing Microsoft DNS
- **Domain Name System (DNS)**
	- TCP/IP application protocol that enables a DNS server to resolve:
		- **Forward lookup** - Domain and computer names to IP addresses
		- **Reverse lookup** - IP addresses to domain and computer names
		- <u>Security implications</u>
			- Refusal for connection from host without reverse lookup record
	- DNS servers provide the DNS namespace for an enterprise
	- One of the requirements for using Active Directory is to have DNS servers set up for its domains
	- Windows Server DNS is the most compatible with the Active Directory
		- Non-Microsoft servers can be used but they must be compatible with the Active Directory
		- Microsoft-based DNS provides additional information to help domain clients to locate DCs and GCs

> **Additional Notes:**
> - DNS provides important operational network services to domain clients
> - Misconfigured or compromised DNS can cause operational and security issues
> - **Example:** Someone can intercept DNS registrations or queries and use them to impersonate users to plant for possible immediate or future attacks

### Installing DNS Services
- DNS is installed as a server role in Windows Server 2016

#### DNS Zones
> - Common type of DNS query is resolving the IP address of host or domain name
> - Based on information stored on the **forward lookup zone**
> - In a scenario when the query is to resolve the host or domain name of a particular IP address, this type of query requires the information from the **reverse lookup zone**
> - **Stub zone** is a special type of internal lookup
> - Both reverse lookup and stub zones must be set up manually

- DNS name resolution enabled through the use of tables of information that links computer names and IP addresses
- Tables are associated with partitions in a DNS server known as **zones**
	- Contains resource records
- **Forward Lookup Zone**
	- Zone that links computer names to IP addresses
	- Holds host name records called address records
	- Most DNS queries handled based on resource records from this zone
- In IPv4, host record is called a host address (A) resource record
- IPv6 record is called an IPv6 host address (AAAA) resource record
- When DNS installed on a domain controller in a domain:
	- Forward lookup zone automatically created for the domain with DNS server address record already entered

| Resource Record                                  | Description                                                                                                                                                |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Host (A)                                         | Links computer or network host name to IP address                                                                                                          |
| Canonical Name (CNAME)                           | Links alias to computer name,  sometimes called a common name                                                                                              |
| Load sharing                                     | Spreads load of DNS lookup requests among multiple DNS servers to provide faster resolution for clients and better network response                        |
| Mail Exchanger (MX)                              | Provides IP address for SMTP servers that can accept email for users in a domain                                                                           |
| Name Server (NS)                                 | Provides information in response to queries about secondary DNS servers for authoritative server and off-site primary servers not authoritative for domain |
| Pointer Record (PTR)                             | Associates IP address to computer or network host name                                                                                                     |
| Service Locator (SRV)                            | Associates particular TCP/IP service to server along with domain of server and its protocol                                                                |
| Start of Authority (SOA)                         | First record in a zone and also indicates if server is authoritative for current zone                                                                      |
| Windows Internet Naming Service (WINS)           | Forwards lookup request for NetBIOS name to WINS server when host name cannot be found in DNS                                                              |
| Windows Internet Naming Service Reverse (WINS-R) | Forwards reverse lookup request to WINS server                                                                                                             |

### Using DNS Dynamic Update Protocol
- Microsoft DNS is also known as Dynamic DNS (DDNS)

> **Dynamic DNS** - Modern form of DNS that enables client computers and DHCP servers to automatically register IP addresses

- **DNS Dynamic Update Protocol**
	- Enables information in DNS server to be  automatically  updated in coordination with DHCP
- After configuring DNS:
	- Ensure that is configured to secure the DNS dynamic update protocol
	- Saves administrators time since they no longer have to manually register each new workstation or each time new IP lease is issued

### DNS Replication
**Primary DNS Server:**
- Main administrative server for a zone and thus also the authoritative server for that zone
**Secondary DNS Server:**
- Contains copy of primary DNS server zone database but not used for administration
- Obtains copy of zone database through a zone transfer over the network

<u>Vital services performed by secondary DNS servers:</u>
- Make sure that there is always a copy of primary DNS server's data
- Enable DNS load balancing among primary DNS server and secondary servers
- Let secondary DNS face and answer to queries from the public network
- Reduce congestion in one part of the network

**If using Active Directory and have two or more DCs:**
- Set up Microsoft DNS services on at least two of the DCs

> **Notes:**
> - Unlike DC Replication, DNS Replication is not working in multi-master mode
> - Zone data updates can only be taken place at the primary DNS
> - Secondary DNS servers unable to fully replace functions of primary DNS
> - Only authorized DNS can request zone data transfer from master
> - Replication process also protects transfer data using both authentication and encryption schemes

### DNS Query
- Client request local DNS for a name to IP address resolution
- <u>Local DNS gives an immediate answer when:</u>
	- The authoritative server for the particular domain
	- Local DNS has the required information from its cache
- <u>When DNS does not have the answer:</u>
	- Looks up IP address of corresponding authoritative server from one of the root servers (root server addresses are built-in to DNS installation)
	- Involves multiple rounds of inquiries
	- Local DNS acts as client to visit multiple authoritative DNS servers to get the final server
	- Also known as **Iterative Lookup**
	- Once the actual authoritative server is identified, local DNS contacts the remote DNS directly and gets required information

### Stub Zone
- Bare necessities for DNS functions
- Copies of the following
	- SOA record zone
	- Name server records to identify authoritative servers
	- Record for name servers that are authoritative
- One common use for a stub zone is help quickly resolve computer names between two different namespaces
- **Zone Transfer Privilege**
	- Local DNS requires to have zone transfer privilege granted from the target DNS that owns the Forward Zone
	- Cannot set up stub zone for `google.com` unless local DNS has required privilege granted from authoritative DNS of `google.com`

### Additional DNS Server Roles
- Common to designate one DNS server to forward name resolution requests to specific remote DNS server
- DNS forwarding can be set up if DNS server receiving the forwarded request cannot resolve the name
	- Server that originally forwarded the request attempts to resolve via other ways
	- Called **nonexclusive forwarding**
- Windows Server 202126 supports use of forwarder and root hints
	- But root hints traffic may be blocked by higher level ISP for security measure

![](https://i.imgur.com/xsDVH19.png)

- DNS server can function as a caching server
>- **Caching server** used to provide fast queries as results of each query are stored in RAM

- DNS server without zones is a server that is caching-only
	- Caching-only DNS server queries primary or secondary DNS server and caches results to provide fast response for the next identical query
	- Used to reduce the number of secondary server and reduce extra network traffic
- **Limitation of using caching servers:**
	- Takes time for each one to build up a comprehensive set of resolved names to IP addresses
- It is sometimes necessary to flush DNS server cache

### Forwarders vs Root Hints
| Forwarders                                                                                       | Root Hints                                                                           |
| ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| Faster (recursive query)                                                                         | Slower due to interactive queries                                                    |
| Local DNS has no way to ensure security configuration of external forwarder                      | More reliable (13 root hints support by more than 200 actual load-balancing servers) |
| Potential to enable customizable DNS service within an enterprise e.g. DNS filtering and logging |                                                                                      |

## Creating DNS Implementation
### Best Practice Recommendations
- Implement Windows Server 2016 DNS servers instead of other versions of DNS and use Active Directory
- Resource records and zones that can be set up for IPv4 can also be set up for IPv6
- Consider using namespaces to represent natural organisational boundaries
- Make sure DNS servers on private network are well secured in terms of Windows Server 2016 security options
- Plan to locate DNS server across most site links
- Create two or more DNS servers to take advantage of load balancing, multi-master relationships (with AD-integrated zones) and fault tolerance
- Designate one DNS server as a forwarder to reduce traffic
- Number of DNS servers set up can be related to analysis or an organisation
- If there are multiple servers used for one application, use round robin to distribute the load
	- Only applicable for namespace with cluster setup
- If a branch location with a **read-only domain controller (RODC)** needs local DNS services because there are many users, make the RODC a secondary DNS server and not a primary DNS server

> **Security Benefits of RDOC**
> - Unidirectional replication (compromised RODC will not spread malicious updates)
> - Not caching any passwords on a RDOC

## DNS Enhancement in Windows Server 2016
- DNS group policies for:
	- Filtering malicious IP address and redirecting malicious clients to a dead end rather than to the computer they want to reach
	- Redirecting clients to specific data sources or servers according to time of the day
	- Managing client access to account for high traffic situations
	- Directing clients to best source for specific application
- Ability for DNS to work with client computers that have more than one NIC
- New PowerShell cmdlets for configuring DNS

## Troubleshooting DNS
- Steps to take to troubleshoot DNS problems
	- Restart the DNS server and DNS client services
	- Check for the most recent log errors relating to DNS
- **DNS Server Problems:**
	- <u>DNS server is not responding with correct information to DNS queries from clients</u>
		- Check manually added DNS host address records for accuracy
		- If errors are related to one client, check the DNS client computer to ensure that it is working correctly including that the NIC and its driver are working correctly
	- <u>Users can contact DNS server but receive a permission denied message when trying to read DNS records</u>
		- Ensure that users have permission to read DNS records, open the DNS Manager, right-click the domain in the tree, click Properties > Security tab and ensure that the Everyone group and other general user groups has *Allow* checked for Read permission
	- <u>Users cannot access the DNS server</u>
		- Check the DNS server's connection to the network to ensure that it is live on the network
	- <u>Users can access the DNS server, but DNS record information is not being processed</u>
		- Ensure that DNS Server and DNS Client services are started and set to start automatically when the server is booted
	- <u>Installed an update to DNS server but DNS response to client is experiencing errors</u>
		- Reload the DNS server database, open the DNS Manager, click the domain in the tree, click the Action menu and click Reload to reload the database

## DNS Threats
- Enterprise Administrators should watch out for the following possible DNS related threats:
	- **DDoS with DNS Amplification Attack**
		- Bogus queries are sent to a DNS to trigger it to send high volume of replies to target victim
	- **Cache Poisoning**
		- DNS Cache / Stub Zone / Forwarder is comprised, causing DNS to return incorrect IP addresses, diverting traffic to attacker's computer
	- **Registrar Hijacking**
		- Also known as **domain hijacking**
		- Act of changing the registration of a domain without permission of its original registrant
		- Can be done in several ways generally by exploiting a vulnerability in the domain name registration system or through social engineering

## Using Microsoft Baseline Security Analyser
> **MBSA** - tool used to scan Windows-based computers for common security holes

- Compares a computer's settings against Microsoft's released patches and listed vulnerabilities
- Runs on Windows Server 2013, Windows Server 2018, Windows Server 2012 R2 and Windows Server 2016
- Most current and last version of MBSA is 2.3
- MBSA provides streamlined method to identify missing security updates and common security configurations
- Microsoft Security Compliance Toolkit (SCT)
	- New set of tools for Windows 10 and Windows Server 2019
- MBSA checks the following desktop application parameters:
	- Internet Explorer security zone settings per local user
	- Outlook security zone settings per local user
	- Office products security zone settings per local user
- MBSA tool to be run on multiple machines or remotely, the following must be enabled:
	- Server Service, Remote Registry Service, File and Print Sharing
	- May disable all these by default and use Group Policies to enable them prior to a routine MBSA scan for the entire network or subnet

## Microsoft Security Compliance Manager
- Utility to enable automation deployment and monitoring baseline compliance
- **Key Feature and Benefits:**
	- Download security baselines from Microsoft
	- Compare and customise security baselines
	- Export baselines in formats like XLS and/or Group Policy Object (GPOs)

## Security Features in Windows Server 2016
- Windows Server 2016 was created to emphasize security:
	- Reduced attack surface of the kernel through Server Core and Nano Server
	- Expanded and evolving Group Policies
	- Windows Firewall
	- Windows Defender
	- Security Templates and Security Configuration and Analysis tools
	- User Account Control
	- BitLocker Drive Encryption
- Server Core or Nano Server can be a good solution for a server that handles critical network operations e.g. DNS and DHCP
	- Also can be a good solution for a web or other server in a DMZ of a network
	- Can also offer better performance and lead to fewer problems

> **Demilitarised Zone (DMZ)** - Portion of a network that is in between two networks e.g. between private network and Internet
> - Computers in the DMZ have fewer security defences via routers and firewalls

- Group policy is a way to bring consistent security and other management to Windows Server 2016 and to clients connecting to a server
- Settings are merged in Windows Firewall and IPSec for consistency

> **Windows Defender** - Software that scans for viruses, spyware and malware
> - Windows Server 2016 is the first Windows Server OS to include Windows Defender

- Security Templates and Security Configuration and Analysis tools that enables users to configure server-wide security
- **User Account Control (UAC)**
	- Designed to keep the user running in the standard user mode as a way to:
		- More fully insulate the kernel
		- Keep operating system and desktop files stabilized
	- Another element of UAC is the Administrator Approval Mode
		- Prevents malware or intruder from acquiring control through a backdoor without administrator's knowledge
- **BitLocker Drive Encryption**
	- Prevents an intruder from bypassing ACL file and folder protections

## Introduction to Group Policy
> Group Policy in Windows Server 2016 enables users to standardize working environment of clients and servers by settings policies in the Active Directory

- **Defining characteristics of Group Policy:**
	- Group Policy can be set for a site, domain, OU or local computer
	- Group Policy cannot be set for non-OU folder containers
	- Group Policy settings are stored in Group Policy Objects
	- GPOs can be local and nonlocal
	- Group Policy can be set up to affect user accounts and computers
	- When Group Policy is updated, old policies are removed or updated for all clients

![](https://i.imgur.com/KJEQWCp.png)

- Computer Configuration settings apply to computers
![](https://i.imgur.com/j3HUbVW.png)

- User Configuration settings applies to users
![](https://i.imgur.com/1zvE7Cu.png)

- Security policies are subset of individual policies
	- Within a larger group policy for a site, domain, OU or local computer
- Security policies include the following:
	- Account Policies
	- Audit Policy
	- User Rights
	- Security Options
	- IP Security Policies

### Account Policies
- Security measures set up in a Group Policy that applies to all accounts or to all accounts in a container when Active Directory is installed
- Account policy options that users can configure affects **three main areas:**
	- Password security
	- Account lockout
	- Kerberos security

#### Password Security
- One option is to set a password expiration period requiring users to change passwords at regular intervals