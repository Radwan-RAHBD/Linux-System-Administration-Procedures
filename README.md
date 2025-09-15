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
- **[Repository Configuration](./Repository_Configuration/Repository_Configuration.md)** ✅ *Complete*
  - Configure YUM/DNF repositories, GPG keys, and package sources

### ⏰ Time Synchronization 
- **[NTP Configuration](./NTP_Configuration/)** ✅ *Complete*
  - [NTP Server Setup](./NTP_Configuration/Server.md) - Configure NTP master server
  - [NTP Client Setup](./NTP_Configuration/Client.md) - Configure NTP client synchronization

### 🌐 Web Server Configuration
- **[HTTP Configuration](./HTTP_Configuration/HTTP_Configuration.md)** ✅ *Complete*
  - Basic Apache HTTP server setup and configuration
- **[HTTPS SSL Certificates](./HTTPS_SSL_Certificates/HTTPS_SSL_Certificates.md)** ✅ *Complete*
  - SSL/TLS certificate installation and configuration
- **[Web Server - Apache Setup](./Web_Server_Apache_Setup/Web_Server_Apache_Setup.md)** ✅ *Complete*
  - Advanced Apache configuration with virtual hosts and security
- **[Web Server - Nginx Setup](./Web_Server_Nginx_Setup/Web_Server_Nginx_Setup.md)** ✅ *Complete*
  - Complete Nginx setup with PHP-FPM integration

### 🖥️ Application & Database Servers
- **[Application Server Setup](./Application_Server_Setup/Application_Server_Setup.md)** ✅ *Complete*
  - Multi-language application server (PHP, Python, Node.js, Java)
- **[Database Server - MySQL](./Database_Server_MySQL/)** 🔄 *In Progress*
  - [MySQL Server](./Database_Server_MySQL/Server.md) - MySQL server installation and configuration
  - [MySQL Client](./Database_Server_MySQL/Client.md) - MySQL client tools and connection setup
- **[Database Server - Oracle](./Database_Server_Oracle/)** 🔄 *In Progress*
  - [Oracle Server](./Database_Server_Oracle/Server.md) - Oracle Database server setup
  - [Oracle Client](./Database_Server_Oracle/Client.md) - Oracle client tools and connectivity

### 🛡️ Security & Access Management
- **[SUDO Management](./SUDO_Management/SUDO_Management.md)** ✅ *Complete*
  - Role-based sudo access configuration and security
- **[SELinux Configuration](./SELinux_Configuration/)** 📋 *Planned*
  - SELinux policy management and troubleshooting

### 🌍 Network Services
- **[DNS Configuration](./DNS_Configuration/)** 🔄 *In Progress*
  - [DNS Server](./DNS_Configuration/Server.md) - Authoritative DNS server setup
  - [DNS Client](./DNS_Configuration/Client.md) - DNS resolver configuration

### ⚙️ System Management
- **[LVM Management](./LVM_Management/)** 📋 *Planned*
  - Logical Volume Manager configuration and operations
- **[Cron Jobs and Automation](./Cron_Jobs_and_Automation/)** 📋 *Planned*
  - Task scheduling and automation procedures

### 💾 Backup & Recovery
- **[Backup Procedures](./Backup_Procedure/)** 📋 *Planned*
  - [Server Backup](./Backup_Procedure/Server.md) - Database and service backup procedures
  - [Client Backup](./Backup_Procedure/Client.md) - File system backup procedures
- **[Restore Procedures](./Restore_Procedure/)** 📋 *Planned*
  - [Server Restore](./Restore_Procedure/Server.md) - Service and database restoration
  - [Client Restore](./Restore_Procedure/Client.md) - File system restoration

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
- **Environment:** Enterprise production systems
- **Use Cases:** Banking, healthcare, telecommunications

---

## 🚀 Quick Start

1. **Choose your procedure** from the categories above
2. **Review prerequisites** in the Preparation section
3. **Follow implementation steps** systematically
4. **Verify configuration** using provided tests
5. **Keep rollback procedures** ready for emergency recovery

---

## 📚 Documentation Standards

- ✅ **Complete procedures** are fully tested and documented
- 🔄 **In Progress** procedures are being developed/refined
- 📋 **Planned** procedures are scheduled for future development
- All procedures include troubleshooting sections
- Enterprise-focused with real-world scenarios

---

## 🤝 Contributing

This repository represents professional system administration procedures developed through hands-on experience in enterprise environments. For questions or suggestions, please contact via the information above.

---

## 📄 License

These procedures are provided for educational and professional use. Please ensure compliance with your organization's change management policies.

---

**Professional Linux System Administration**  
*Radwan Awad - Database Developer & Oracle DBA*
