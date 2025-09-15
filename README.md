# Linux System Administration Procedures

**Professional Change Management Procedures for AlmaLinux/RHEL 8**

![Linux](https://img.shields.io/badge/Linux-AlmaLinux%208-blue.svg)
![RHEL](https://img.shields.io/badge/RHEL-8-red.svg)
![Status](https://img.shields.io/badge/Status-Active%20Development-green.svg)

---

## 📋 Overview

This repository contains comprehensive Change Management Procedures for Linux system administration, specifically designed for AlmaLinux and RHEL 8 environments. Each procedure follows a standardized 4-section approach: **Preparation → Implementation → Verification → Rollback**.

**Author:** Radwan Awad  
**Website:** RaHBD.com  
**Contact:** radwan@RaHBD.com  

---

## 🗂️ Procedure Categories

### 📊 Repository & Package Management
- **[Repository Configuration](./Repository_Configuration.md)** ✅ *Complete*
  - Configure YUM/DNF repositories, GPG keys, and package sources

### ⏰ Time Synchronization 
- **[NTP Configuration](./Procedures/NTP_Configuration/)** ✅ *Complete*
  - [NTP Server Setup](./Procedures/NTP_Configuration/Server.md) - Configure NTP master server
  - [NTP Client Setup](./Procedures/NTP_Configuration/Client.md) - Configure NTP client synchronization

### 🌐 Web Server Configuration
- **[HTTP Configuration](./Procedures/HTTP_Configuration.md)** 
  - Basic Apache HTTP server setup and configuration
- **[HTTPS SSL Certificates](./Procedures/HTTPS_SSL_Certificates.md)** 
  - SSL/TLS certificate installation and configuration
- **[Web Server - Apache Setup](./Procedures/Web_Server_Apache_Setup.md)** 
  - Advanced Apache configuration with virtual hosts and security
- **[Web Server - Nginx Setup](./Procedures/Web_Server_Nginx_Setup.md)** 
  - Complete Nginx setup with PHP-FPM integration
- **[Application Server Setup](./Procedures/Application_Server_Setup.md)** 
  - Multi-language application server (PHP, Python, Node.js, Java)

### 🛡️ Security & Access Management
- **[SUDO Management](./Procedures/SUDO_Management.md)** 
  - Role-based sudo access configuration and security

### 🌍 Network Services
- **[DNS Configuration](./Procedures/DNS_Configuration/)** 
  - [DNS Server](./Procedures/DNS_Configuration/Server.md) - Authoritative DNS server setup
  - [DNS Client](./Procedures/DNS_Configuration/Client.md) - DNS resolver configuration

### 🗄️ Database Servers
- **[Database Server - MySQL](./Procedures/Database_Server_MySQL/)** 
  - [MySQL Server](./Procedures/Database_Server_MySQL/Server.md) - MySQL server installation and configuration
  - [MySQL Client](./Procedures/Database_Server_MySQL/Client.md) - MySQL client tools and connection setup
- **[Database Server - Oracle](./Procedures/Database_Server_Oracle/)** 
  - [Oracle Server](./Procedures/Database_Server_Oracle/Server.md) - Oracle Database server setup
  - [Oracle Client](./Procedures/Database_Server_Oracle/Client.md) - Oracle client tools and connectivity

### 📧 Mail Server Configuration
- **[Mail Server](./Procedures/Mail_Server/)** 
  - [SMTP Server - Postfix](./Procedures/Mail_Server/SMTP_Server_Postfix.md) - Complete Postfix SMTP server setup
  - [SMTP Client - Postfix](./Procedures/Mail_Server/SMTP_Client_Postfix.md) - Postfix client configuration
  - [Mail Server - Dovecot](./Procedures/Mail_Server/Mail_Server_Dovecot.md) - Dovecot IMAP/POP3 server setup
  - [Mail Client - Dovecot](./Procedures/Mail_Server/Mail_Client_Dovecot.md) - Dovecot client configuration
  - [Mail Server Integration](./Procedures/Mail_Server/Mail_Server_Integration.md) - Complete mail system integration
- **[iRedMail Server](./Procedures/iRedMail_Server.md)** 
  - Complete iRedMail installation and configuration for enterprise email systems

---

## 🎯 Procedure Structure

Each procedure follows this standardized format:

### 1. **Preparation**
- Prerequisites and requirements
- Environment setup and backup procedures
- Required information gathering

### 2. **Implementation Steps**
- Step-by-step configuration commands
- Complete code examples and configurations
- Security considerations

### 3. **Verification**
- Testing and validation procedures
- Performance monitoring
- Troubleshooting verification

### 4. **Rollback**
- Recovery procedures
- Configuration restoration
- Emergency fallback options

---

## 🛠️ Target Environment

- **Operating System:** AlmaLinux 8 / RHEL 8
- **Architecture:** x86_64
- **Use Cases:** System administration and infrastructure management

---

## 🚀 Quick Start

1. **Choose your procedure** from the categories above
2. **Review prerequisites** in the Preparation section
3. **Follow implementation steps** systematically
4. **Verify configuration** using provided tests
5. **Keep rollback procedures** ready for emergency recovery



---

**Professional Linux System Administration**  
*Radwan Awad - System Administrator*
