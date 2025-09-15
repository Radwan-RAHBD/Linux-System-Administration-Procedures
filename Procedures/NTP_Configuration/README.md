# NTP Configuration - Implementation Guide

**Author: Radwan Awad**  
**Website: RaHBD.com**  
**Email: radwan@RaHBD.com**

## Overview

This directory contains NTP (Network Time Protocol) configuration procedures for AlmaLinux/RHEL 8 systems. There are two separate files because each system has a different role in the NTP network hierarchy.

## File Structure

```
NTP_Configuration/
├── README.md          # This implementation guide
├── Server.md          # NTP Server setup procedure
└── Client.md          # NTP Client setup procedure
```

## Implementation Order

### Phase 1: Server Setup First
**🔄 Start with Server.md**

The NTP server must be configured and running before clients can synchronize with it.

**Why Server First?**
- Server provides time reference for entire network
- Clients need a working server to connect to
- Server synchronizes with external time sources first

### Phase 2: Client Setup Second  
**🔄 Then proceed with Client.md**

Configure clients only after server is verified and working properly.

**Why Client Second?**
- Clients depend on server being available
- Easier to troubleshoot when server is stable
- Can test connectivity to known working server

## Key Differences Between Server and Client

### Server Configuration (Server.md)
- **Purpose**: Acts as time source for network clients
- **Network Role**: Listens for incoming NTP requests
- **Firewall**: Opens UDP port 123 for client connections
- **Configuration**: Includes `allow` directives for client networks
- **Commands**: Can use `chronyc serverstats` to monitor clients
- **Dependencies**: Connects to upstream public NTP servers

### Client Configuration (Client.md)
- **Purpose**: Synchronizes system time with NTP servers
- **Network Role**: Makes outgoing connections only
- **Firewall**: No incoming ports needed
- **Configuration**: Points to specific NTP servers (internal + backup)
- **Commands**: Uses `chronyc tracking` to monitor sync status
- **Dependencies**: Requires working NTP server(s) to connect to

## Quick Reference Commands

### For Server Verification
```bash
chronyc clients          # Show connected clients
chronyc serverstats      # Server performance statistics
systemctl status chronyd # Service status
```

### For Client Verification
```bash
chronyc sources -v       # Show time sources and status
chronyc tracking         # Current synchronization info
timedatectl status       # System time and NTP status
```

## Common Deployment Scenarios

### Scenario 1: Single NTP Server
1. Deploy one NTP Server using Server.md
2. Configure all other systems as clients using Client.md

### Scenario 2: Multiple NTP Servers (Redundancy)
1. Deploy 2-3 NTP Servers using Server.md
2. Configure clients to use multiple servers
3. Clients will automatically select best server

### Scenario 3: Hierarchical Setup
1. Deploy primary NTP Server (Stratum 2) using Server.md
2. Deploy secondary NTP Servers pointing to primary
3. Configure clients to use secondary servers

## Troubleshooting Priority

1. **Always check server first** if clients have sync issues
2. **Verify network connectivity** between client and server
3. **Check firewall rules** on both server and client
4. **Monitor logs** on both systems: `/var/log/chrony/`