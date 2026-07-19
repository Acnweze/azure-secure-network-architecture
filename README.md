# Azure Network Security Architecture Project

## Project Overview

This project demonstrates a secure Azure network architecture using a segmented application design.

The environment consists of:

* A public-facing Web Server VM
* A private Database VM
* Network Security Groups (NSGs) controlling traffic flow
* Secure administrative access through restricted SSH rules
* Validation tests proving allowed and blocked traffic

The goal was to implement a real-world cloud security model where only required communication paths are permitted.

---

# Architecture Design

```text
                         Internet
                            |
                            |
                 HTTP (80) / HTTPS (443)
                            |
                            ▼
                 CONTOSO-WEB-01
                 Ubuntu 24.04
                 Private IP: 10.30.1.4
                 Public IP: Enabled
                            |
                            |
                    MySQL TCP 3306
                            |
                            ▼
                 CONTOSO-DB-01
                 Ubuntu 24.04
                 Private IP: 10.30.2.4
                 Public IP: Removed
```

---

# Azure Resources

## Resource Group

Resource group used:

* RG-CONTOSO-SECURITY

![Resource Group](screenshots/01-resource-group.png)

---

## Virtual Machines

Deployed:

| VM             | Role            | Subnet          |
| -------------- | --------------- | --------------- |
| CONTOSO-WEB-01 | Web Server      | Web-Subnet      |
| CONTOSO-DB-01  | Database Server | Database-Subnet |

![Virtual Machines](screenshots/02-virtual-machines.png)

---

# Network Design

## Virtual Network

VNet:

* VNET-Contoso-Secure

Subnets:

* Web-Subnet
* Database-Subnet

![VNet Subnets](screenshots/03-vnet-subnets.png)

---

# Web Server Configuration

## SSH Access

Connected to the Web VM through restricted SSH access.

![SSH Web VM](screenshots/04-ssh-web-vm.png)

---

## Nginx Deployment

Installed and configured Nginx as the web server.

Service status:

![Nginx Running](screenshots/05-nginx-service-running.png)

Web page validation:

![Nginx Web Page](screenshots/06-nginx-web-page.png)

---

# Database Server Configuration

## Database VM Access

Connected to the private database server.

![SSH Database VM](screenshots/07-ssh-db-vm.png)

---

## MySQL Installation

MySQL was installed and configured on the database server.

![MySQL Running](screenshots/10-mysql-service-running.png)

---

## MySQL Network Configuration

Updated MySQL binding to allow private network communication.

![MySQL Network Config](screenshots/11-mysql-network-config.png)

---

# Network Security Groups (NSGs)

## Web Tier NSG

Implemented rules to control:

* HTTP access
* HTTPS access
* Restricted SSH administration

![Web NSG Rules](screenshots/13-web-nsg-rules.png)

SSH access was limited to the administrator public IP.

![SSH Restriction](screenshots/14-web-nsg-ssh-restriction.png)

---

## Database Tier NSG

Database access was restricted to required traffic only.

![Database NSG Rules](screenshots/15-database-nsg-rules.png)

---

# Security Testing

## Test 1 — Web Server to Database Connection

Allowed traffic:

```text
Source:
CONTOSO-WEB-01
10.30.1.4

Destination:
CONTOSO-DB-01
10.30.2.4

Protocol:
TCP

Port:
3306
```

Result:

Successful MySQL connection

![Web Database Connection](screenshots/16-web-to-database-success.png)

---

## Test 2 — MySQL User Restriction

Created a dedicated database user:

```text
webapp@10.30.1.%
```

This limits database access to the Web subnet only.

![MySQL Webapp User](screenshots/17-mysql-webapp-user.png)

---

## Test 3 — Database to Web HTTP Block

Attempted:

```bash
curl http://10.30.1.4
```

Expected:

Blocked

Result:

Traffic denied by NSG rules

![Blocked Traffic Test](screenshots/18-blocked-database-to-web-test.png)

---

# Database Security Improvement

Removed the public IP from the Database VM.

Before:

```text
Database VM
Public IP: Enabled
```

After:

```text
Database VM
Private IP Only
10.30.2.4
```

![Database Public IP Removed](screenshots/19-database-public-ip-removed.png)

---

# SSH Security Validation

Updated SSH access after administrator IP change.

![SSH Admin IP Update](screenshots/20-ssh-admin-ip-updated.png)

---

# Security Controls Implemented

* Network segmentation using Azure subnets
* Web tier separated from database tier
* NSG-based traffic filtering
* Restricted SSH administration
* Database private access only
* MySQL access limited to Web subnet
* Blocked unauthorized communication paths
* Tested security rules with real traffic validation

---

# Key Learnings

Through this project, I practiced:

* Azure Virtual Network design
* Subnet segmentation
* Network Security Group configuration
* Linux VM administration
* Nginx deployment
* MySQL server configuration
* Secure cloud architecture principles
* Security testing and validation

---

# Project Status

Completed

This project demonstrates a production-style Azure network security architecture with controlled access, segmentation, and validated security rules.
