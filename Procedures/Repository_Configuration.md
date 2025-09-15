# Repository Configuration - AlmaLinux/RHEL 8

**Change Management Procedure**

**Author: Radwan Awad**  
**Website: RaHBD.com**  
**Email: radwan@RaHBD.com**

## 1. Preparation

### Prerequisites
- Root access to the target system
- Network connectivity to repository servers
- Current repository configuration backup
- System update planned (repositories will be refreshed)

### Environment Setup

```bash
# Create backup directory
mkdir -p /backup/repo_config_$(date +%Y%m%d)

# Backup current repository configuration
cp -r /etc/yum.repos.d/ /backup/repo_config_$(date +%Y%m%d)/
cp /etc/yum.conf /backup/repo_config_$(date +%Y%m%d)/

# Check current repositories
yum repolist all > /backup/repo_config_$(date +%Y%m%d)/current_repos.txt
```

### Required Information
- Repository URLs and mirror lists
- GPG keys for repository verification
- Network proxy settings (if applicable)
- Local repository server details (if using internal mirrors)

## 2. Implementation Steps

### Step 1: Clean Current Repository Cache

```bash
# Clean all repository metadata
yum clean all

# Remove cached repository data
rm -rf /var/cache/yum/*
```

### Step 2: Configure Base AlmaLinux Repositories

```bash
# Create AlmaLinux base repository file
cat > /etc/yum.repos.d/almalinux.repo << 'EOF'
[baseos]
name=AlmaLinux $releasever - BaseOS
mirrorlist=https://mirrors.almalinux.org/mirrorlist/$releasever/baseos
enabled=1
gpgcheck=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux

[appstream]
name=AlmaLinux $releasever - AppStream
mirrorlist=https://mirrors.almalinux.org/mirrorlist/$releasever/appstream
enabled=1
gpgcheck=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux

[extras]
name=AlmaLinux $releasever - Extras
mirrorlist=https://mirrors.almalinux.org/mirrorlist/$releasever/extras
enabled=1
gpgcheck=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux
EOF
```

### Step 3: Import GPG Keys

```bash
# Import AlmaLinux GPG key
rpm --import https://repo.almalinux.org/almalinux/RPM-GPG-KEY-AlmaLinux

# Verify GPG key imported
rpm -qa | grep gpg-pubkey
```

### Step 4: Configure EPEL Repository

```bash
# Install EPEL repository
yum install -y epel-release

# Verify EPEL repository configuration
cat /etc/yum.repos.d/epel.repo
```

### Step 5: Configure Additional Repositories (Optional)

```bash
# PowerTools repository (needed for development packages)
yum config-manager --set-enabled powertools

# Or manually enable PowerTools
sed -i 's/enabled=0/enabled=1/' /etc/yum.repos.d/almalinux-powertools.repo
```

### Step 6: Configure Local/Internal Repository (if applicable)

```bash
# Create internal repository configuration
cat > /etc/yum.repos.d/internal.repo << 'EOF'
[internal-base]
name=Internal Repository - Base
baseurl=http://internal-repo.company.com/almalinux/8/base/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux

[internal-updates]
name=Internal Repository - Updates
baseurl=http://internal-repo.company.com/almalinux/8/updates/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux
EOF
```

### Step 7: Update Repository Metadata

```bash
# Generate repository cache
yum makecache

# Update repository metadata
yum check-update
```

## 3. Verification

### Repository Status Check

```bash
# List all enabled repositories
yum repolist enabled

# Check repository status
yum repoinfo

# Verify specific repositories are accessible
yum --enablerepo=baseos list available | head -10
yum --enablerepo=appstream list available | head -10
yum --enablerepo=epel list available | head -10
```

### Connectivity Test

```bash
# Test repository connectivity
curl -I https://mirrors.almalinux.org/mirrorlist/8/baseos
curl -I https://download.fedoraproject.org/pub/epel/8/Everything/x86_64/

# Test package installation
yum install -y tree htop
yum remove -y tree htop
```

### GPG Verification Test

```bash
# Verify GPG key functionality
yum install -y --nogpgcheck nano
yum remove -y nano

# Install with GPG check (should work)
yum install -y nano
yum remove -y nano
```

### Repository Priority Check

```bash
# Check repository priorities
yum --showduplicates list available kernel | head -20

# Verify no conflicts between repositories
package-cleanup --problems
```

## 4. Rollback

### Quick Rollback

```bash
# Stop any running yum processes
killall yum

# Restore original repository configuration
rm -f /etc/yum.repos.d/*.repo
cp /backup/repo_config_$(date +%Y%m%d)/*.repo /etc/yum.repos.d/
cp /backup/repo_config_$(date +%Y%m%d)/yum.conf /etc/

# Clear cache and rebuild
yum clean all
yum makecache
```

### Complete System Recovery

```bash
# If system is broken due to repository issues
# Boot into rescue mode or use installation media

# Mount file systems
mount /dev/sda1 /mnt
mount --bind /proc /mnt/proc
mount --bind /sys /mnt/sys
mount --bind /dev /mnt/dev
chroot /mnt

# Restore repository configuration
cp /backup/repo_config_*/yum.repos.d/* /etc/yum.repos.d/
yum clean all
yum makecache

# Exit chroot and reboot
exit
umount /mnt/dev /mnt/sys /mnt/proc /mnt
reboot
```

### Emergency Package Installation

```bash
# If repositories are completely broken but you have RPM files
rpm -ivh --force --nodeps package.rpm

# Download RPM manually and install
wget http://mirror.centos.org/centos/8/BaseOS/x86_64/os/Packages/package.rpm
rpm -ivh package.rpm
```

## Common Issues & Solutions

### Repository metadata download failed
- Check network connectivity: `ping mirrors.almalinux.org`
- Verify firewall rules allow HTTP/HTTPS traffic
- Check proxy settings in `/etc/yum.conf`

### GPG key verification errors
- Re-import GPG keys: `rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux`
- Temporarily disable GPG check: `yum install --nogpgcheck package`

### Slow repository access
- Use faster mirrors by editing repository files
- Configure local repository mirror
- Use fastestmirror plugin

### Conflicting packages between repositories
- Set repository priorities using priorities plugin
- Exclude problematic packages: `exclude=package_name` in repo file
- Use `--enablerepo` and `--disablerepo` options selectively 
