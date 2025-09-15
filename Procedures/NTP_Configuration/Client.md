# NTP Configuration - Client Setup - AlmaLinux/RHEL 8

**Change Management Procedure**

**Author: Radwan Awad**  
**Website: RaHBD.com**  
**Email: radwan@RaHBD.com**

## 1. Preparation

### Prerequisites
- Root access to the target system
- Network connectivity to NTP servers
- Knowledge of organizational NTP server addresses
- System time should be roughly correct (within a few hours)

### Environment Setup

```bash
# Create backup directory
mkdir -p /backup/ntp_client_config_$(date +%Y%m%d)

# Backup current time configuration
cp /etc/chrony.conf /backup/ntp_client_config_$(date +%Y%m%d)/
systemctl status chronyd > /backup/ntp_client_config_$(date +%Y%m%d)/chronyd_status.txt
timedatectl status > /backup/ntp_client_config_$(date +%Y%m%d)/timedatectl_status.txt

# Check current time sources
chrony sources > /backup/ntp_client_config_$(date +%Y%m%d)/current_sources.txt
```

### Required Information
- Internal NTP server IP addresses
- Backup/public NTP server addresses
- Timezone information
- Network connectivity requirements

## 2. Implementation Steps

### Step 1: Install Chrony Client

```bash
# Install chrony (usually pre-installed on RHEL 8)
yum install -y chrony

# Stop chronyd service temporarily
systemctl stop chronyd
```

### Step 2: Configure NTP Client Settings

```bash
# Create NTP client configuration
cat > /etc/chrony.conf << 'EOF'
# Use internal NTP servers (replace with your server IPs)
server 192.168.1.10 iburst prefer
server 192.168.1.11 iburst

# Use public NTP servers as backup
server 0.rhel.pool.ntp.org iburst
server 1.rhel.pool.ntp.org iburst

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
minsources 1

# Get TAI-UTC offset and leap seconds from the system tz database
leapsectz right/UTC

# Specify file containing keys for NTP authentication
#keyfile /etc/chrony.keys

# Disable serving time to other clients (client-only mode)
#port 0

# Log client activity
logdir /var/log/chrony
log statistics tracking
EOF
```

### Step 3: Set System Timezone

```bash
# List available timezones
timedatectl list-timezones | grep -i asia  # Example for Asia region

# Set timezone (example: Asia/Riyadh)
timedatectl set-timezone Asia/Riyadh

# Verify timezone setting
timedatectl status
```

### Step 4: Enable NTP Synchronization

```bash
# Enable NTP synchronization
timedatectl set-ntp true

# Verify NTP is enabled
timedatectl status | grep "NTP service"
```

### Step 5: Start and Enable Chrony Service

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

# Verify NTP synchronization is active
timedatectl status | grep "NTP service"
```

### Time Synchronization Verification

```bash
# Check current time sources and their status
chronyc sources -v

# Display detailed tracking information
chronyc tracking

# Show synchronization statistics
chronyc sourcestats

# Verify system clock status
timedatectl status
```

### Network Connectivity Test

```bash
# Test connectivity to configured NTP servers
chronyc activity

# Manual test to NTP server
ntpdate -q 192.168.1.10  # Replace with your server IP

# Check if getting responses from servers
chronyc serverstats 2>/dev/null || echo "Client-only mode - no server stats"
```

### Time Accuracy Check

```bash
# Compare system time with reliable source
date && chronyc tracking | grep "System time"

# Check how often clock is being adjusted
grep "System clock" /var/log/chrony/statistics.log | tail -10

# Monitor ongoing synchronization
watch -n 5 'chronyc sources'
```

## 4. Rollback

### Quick Rollback

```bash
# Stop chronyd service
systemctl stop chronyd

# Restore original configuration
cp /backup/ntp_client_config_$(date +%Y%m%d)/chrony.conf /etc/

# Restart service with original config
systemctl start chronyd

# Verify rollback
chronyc sources
```

### Disable NTP and Set Manual Time

```bash
# Disable NTP synchronization
timedatectl set-ntp false

# Set time manually if needed (example)
timedatectl set-time "2024-03-15 10:30:00"

# Verify manual time setting
timedatectl status
```

### Complete Service Recovery

```bash
# If chronyd fails to start
systemctl stop chronyd

# Reset to default configuration
yum reinstall -y chrony

# Restore working backup configuration
cp /backup/ntp_client_config_$(date +%Y%m%d)/chrony.conf /etc/

# Remove problematic drift file
rm -f /var/lib/chrony/drift

# Restart service
systemctl start chronyd
systemctl enable chronyd

# Re-enable NTP
timedatectl set-ntp true
```

### Fallback to Public NTP Servers

```bash
# If internal servers are unavailable, use public servers only
cat > /etc/chrony.conf << 'EOF'
server 0.rhel.pool.ntp.org iburst
server 1.rhel.pool.ntp.org iburst
server 2.rhel.pool.ntp.org iburst
server 3.rhel.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
leapsectz right/UTC
EOF

# Restart service
systemctl restart chronyd
```

## Common Issues & Solutions

### Cannot synchronize with internal NTP servers
- Test connectivity: `ping internal_ntp_server_ip`
- Check firewall rules allow outgoing NTP (UDP 123)
- Verify server allows this client: contact NTP server administrator
- Switch to public servers temporarily: edit `/etc/chrony.conf`

### Large time difference prevents synchronization
- Check makestep threshold in `/etc/chrony.conf`
- Force step: `chronyc makestep`
- Temporarily set manual time close to correct time
- Increase makestep parameters: `makestep 60.0 -1` for unlimited corrections

### Chronyd service fails to start
- Check configuration syntax: `chronyd -n -d -s`
- Review logs: `journalctl -u chronyd`
- Verify chrony.conf permissions: `ls -l /etc/chrony.conf`
- Test with minimal configuration

### Time drifts frequently
- Check system load and hardware stability
- Verify stable network connection to time servers
- Monitor drift values: `cat /var/lib/chrony/drift`
- Consider hardware RTC issues

### NTP service disabled unexpectedly
- Check if other time services are interfering
- Verify systemd-timesyncd is disabled: `systemctl status systemd-timesyncd`
- Ensure timedatectl NTP setting: `timedatectl set-ntp true`
- Check for conflicting cron jobs adjusting time