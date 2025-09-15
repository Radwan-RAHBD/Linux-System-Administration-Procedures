# NTP Configuration - Server Setup - AlmaLinux/RHEL 8

**Change Management Procedure**

**Author: Radwan Awad**  
**Website: RaHBD.com**  
**Email: radwan@RaHBD.com**

## 1. Preparation

### Prerequisites
- Root access to the target system
- Network connectivity to upstream NTP servers
- Firewall configuration access
- System time should be roughly correct (within a few hours)

### Environment Setup

```bash
# Create backup directory
mkdir -p /backup/ntp_config_$(date +%Y%m%d)

# Backup current time configuration
cp /etc/chrony.conf /backup/ntp_config_$(date +%Y%m%d)/
systemctl status chronyd > /backup/ntp_config_$(date +%Y%m%d)/chronyd_status.txt
timedatectl status > /backup/ntp_config_$(date +%Y%m%d)/timedatectl_status.txt

# Check current time sources
chrony sources > /backup/ntp_config_$(date +%Y%m%d)/current_sources.txt
```

### Required Information
- Upstream NTP server addresses (public or organizational)
- Network subnet ranges that will sync from this server
- Timezone information
- Firewall rules for NTP traffic (UDP port 123)

## 2. Implementation Steps

### Step 1: Install and Configure Chrony

```bash
# Install chrony (usually pre-installed on RHEL 8)
yum install -y chrony

# Stop chronyd service temporarily
systemctl stop chronyd
```

### Step 2: Configure NTP Server Settings

```bash
# Create NTP server configuration
cat > /etc/chrony.conf << 'EOF'
# Use public NTP servers as upstream sources
server 0.rhel.pool.ntp.org iburst
server 1.rhel.pool.ntp.org iburst
server 2.rhel.pool.ntp.org iburst
server 3.rhel.pool.ntp.org iburst

# Allow clients from local network to synchronize
allow 192.168.0.0/16
allow 172.16.0.0/12
allow 10.0.0.0/8

# Serve time even if not synchronized to upstream
local stratum 8

# Record the rate at which the system clock gains/loses time
driftfile /var/lib/chrony/drift

# Allow the system clock to be stepped in the first three updates
makestep 1.0 3

# Enable kernel synchronization of the real-time clock (RTC)
rtcsync

# Enable hardware timestamping on all interfaces that support it
#hwtimestamp *

# Increase the minimum number of selectable sources required to adjust
# the system clock
minsources 2

# Allow NTP client access from local network
bindaddress 0.0.0.0

# Specify file containing keys for NTP authentication
#keyfile /etc/chrony.keys

# Get TAI-UTC offset and leap seconds from the system tz database
leapsectz right/UTC

# Log statistics and tracking information
logdir /var/log/chrony
log statistics measurements tracking
EOF
```

### Step 3: Configure Firewall Rules

```bash
# Allow NTP traffic through firewall
firewall-cmd --permanent --add-service=ntp
firewall-cmd --reload

# Verify firewall rule
firewall-cmd --list-services | grep ntp
```

### Step 4: Set System Timezone

```bash
# List available timezones
timedatectl list-timezones | grep -i asia  # Example for Asia

# Set timezone (example: Asia/Riyadh)
timedatectl set-timezone Asia/Riyadh

# Verify timezone setting
timedatectl status
```

### Step 5: Start and Enable NTP Service

```bash
# Start chronyd service
systemctl start chronyd

# Enable service to start on boot
systemctl enable chronyd

# Check service status
systemctl status chronyd
```

### Step 6: Force Initial Time Synchronization

```bash
# Force chronyd to make an initial time adjustment
chronyc makestep

# Wait for initial synchronization
sleep 30

# Check synchronization status
chronyc tracking
```

## 3. Verification

### Service Status Check

```bash
# Verify chronyd service is running
systemctl status chronyd

# Check if service is enabled
systemctl is-enabled chronyd

# Verify listening on NTP port
netstat -unl | grep :123
```

### Time Synchronization Verification

```bash
# Check current time sources
chronyc sources -v

# Display system clock status
chronyc tracking

# Show statistics about collected measurements
chronyc sourcestats

# Check system time status
timedatectl status
```

### Client Connectivity Test

```bash
# Test NTP server from another machine
ntpdate -q your_server_ip

# Or using chronyc from client
chronyc tracking

# Check server statistics
chronyc serverstats
```

### Network Configuration Test

```bash
# Verify firewall allows NTP
nmap -p 123 -sU localhost

# Test connectivity to upstream servers
chronyc activity

# Check allowed client networks
grep "allow" /etc/chrony.conf
```

## 4. Rollback

### Quick Rollback

```bash
# Stop chronyd service
systemctl stop chronyd

# Restore original configuration
cp /backup/ntp_config_$(date +%Y%m%d)/chrony.conf /etc/

# Restart service with original config
systemctl start chronyd

# Verify rollback
chronyc sources
```

### Complete Service Recovery

```bash
# If chronyd fails to start
systemctl stop chronyd

# Reset to default configuration
yum reinstall -y chrony

# Restore backup configuration
cp /backup/ntp_config_$(date +%Y%m%d)/chrony.conf /etc/

# Remove problematic drift file
rm -f /var/lib/chrony/drift

# Restart service
systemctl start chronyd
systemctl enable chronyd
```

### Emergency Time Setting

```bash
# Manual time setting if NTP fails completely
timedatectl set-ntp false

# Set time manually (example)
timedatectl set-time "2024-03-15 10:30:00"

# Re-enable NTP
timedatectl set-ntp true

# Verify manual setting
timedatectl status
```

### Firewall Rollback

```bash
# Remove NTP firewall rule if needed
firewall-cmd --permanent --remove-service=ntp
firewall-cmd --reload

# Verify removal
firewall-cmd --list-services
```

## Common Issues & Solutions

### Chronyd fails to synchronize with upstream servers
- Check network connectivity: `ping 0.rhel.pool.ntp.org`
- Verify firewall allows outgoing NTP: `firewall-cmd --list-services`
- Check upstream server accessibility: `chronyc activity`

### Large time offset preventing synchronization
- Force step adjustment: `chronyc makestep`
- Increase makestep threshold in `/etc/chrony.conf`: `makestep 10.0 3`
- Manually set approximate time before starting chronyd

### Clients cannot sync to server
- Verify server allows client networks: `grep allow /etc/chrony.conf`
- Check firewall on server: `firewall-cmd --list-services | grep ntp`
- Test server response: `ntpdate -q server_ip` from client

### High time drift or instability
- Check system load and hardware issues
- Verify consistent upstream time sources
- Monitor drift file: `cat /var/lib/chrony/drift`
- Consider hardware clock issues: check BIOS/UEFI settings

### Service fails to start
- Check configuration syntax: `chronyd -n -d -s`
- Verify log files: `tail -f /var/log/chrony/*.log`
- Reset configuration to minimal working state
- Check SELinux denials: `ausearch -m avc -ts recent | grep chronyd`