---
title: "Local Enumeration"
tags:
- enumeration
- windows
- linux
- hacking
---
# User Enum
`whoami` - Get Username
## Windows
`net user` - Show other users
`net user <USERNAME>`  - show detailed \<USERNAME\> information
`net user <USERNAME> /domain` - List all domain users

## Linux
`id` - Get User Identifier(uid) and Group Identifier(gid) of current user

`cat /etc/passwd` - Show all users.

### Hostname Enum
Infer knowledge based on the hostname. Naming conventions can lead to discovery of other machines (i.e. `host23` potentially indicates `host1-22`). OS name, or purpose of the machine may be revealed (i.e. `mailserver`).

`hostname` - get hostname of machine

# System Architecture
## Windows
`systeminfo` - Detailed information on OS and Architecture

To quickly get only OS name, Version and System Type:
```
> systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
OS Name: Microsoft Windows 10 Pro
OS Version: 10.0.16299 N/A 16299
System Type: x86-based PC
```

### Hot fixes and Patches
Take note of potentially un-patched systems. Some older exploits may still work
`wmic qfe get Caption, Description, HotFixID, InstalledOn`

## Linux
`cat /etc/*-release` - Output Distribution name and version

```
$ uname -a
Linux debian 4.9.0-6-686 #1 SMP Debian 4.9.82-1+deb9u3 (2018-03-02) i686 GNU/Linux
```
Kernal version is `4.9.0-6-686`. Architecture is `i86`

# Process enumeration
Investigating currently running processes may hint towards privilege escalation opportunities. Take note of processes/services that were not exposed during port scans and those running as `root` or other privileged accounts.

## Windows
`tasklist /SVC` - Returns processes mapped to specific Windows services. (**Does not list processes run by privileged users**)

## Linux
`ps aux` - Show All processes in User Readable Format with or without a TTY

# Network Information
It's necessary to check available network interfaces, routes, and open ports. Target may be connected to multiple networks and can therefore be used as a pivot to those networks. Certain virtual inferfaces may indicate virtualization or AV software. Port bindings can show if a specific service is only available as a loopback address and therefore reachable only from within the machine. Network connections may reveal other users that could serve as potential future targets.

## Windows
`ipconfig /all` - Display the full TCP/IP configuration of all adapters.

`route print` - Display routing table.

`netstat -ano` - Reveal current active network connections. All tcp connections, Numerical form of address and port, and the Owner of each connection. 

## Linux
`ip a` or `ifconfig a` - Display the full TCP/IP configuration of all adapters.

`route` or `routel` - Display the routing table

`netstat -anp` or `ss -anp` - Display current active network connections. All connections, No hostname resolution, Process names.

# Firewall Status
Firewall rules are more useful during the remote exploitation phase. Some rules that block outgoing traffic for services may reveal it being accessible via loopback interface. Interacting locally may lead to privilege escalation.
Inbound and outbound port filtering helps facilitate port forwarding and tunneling when pivoting.

## Windows
`netsh advfirewall show currentprofile` - Show current firewall profile
`netsh advfirewall firewall show rule name=all` - List firewall rules

## Linux
Listing firewall rules requires `root`
`etc/iptables` directory lists rules to be restored on system boot. If permissions are weak, users may be able to read the files and disclose firewall rules.

# Scheduled Tasks
Servers may periodically execute automated tasks. When misconfigured, or if insufficient permissions are used, files executed by the scheduler at a higher privilege level could lead to privilege escalation.

## Windows
`schtasks /query /fo LIST /v` - Displays scheduled tasks. Lists them in a simple format Verbosely.

## Linux
`ls -lah /etc/cron*` - Lists individual scheduled job tasks.
`cat /etc/crontab` - If able to be read, may contain specific administrative tasks.

# Installed Applications
Outside of system exploits, installed applications may lead to privilege escalation. It's important to note the version of each to determine potential vulnerability.

## Windows
`wmic product get name, version, vendor` - Gets the specified values for applications installed using the windows installer.

## Linux
#### Debian-based Distributions
`dpkg -l` - List installed applications (packages)
#### Red Hat-based Distributions
`rpm -qa` - List installed applications (packages)

# Readable/Writable Files and Directories
Insufficient access restrictions can create a vulnerability that an attacker may leverage to escalate privileges. Sensitive files may contain hardcoded credentials for various services.

## Windows

#### accesschk.exe
A tool used to search the filesystem for files or directories with specific parameters.
`accesschk.exe <FLAGS> <USERNAME> <SEARCH DIRECTORY>`
`accesschk.exe -uws "Everyone" "C:\Program Files"` - Browses file system for files with specific parameters. sUppress errors, search for Write permissions, and perform a recursive Search.

#### Powershell
`Get-Acl` - retrieves permissionf for a given file or directory but cannot be run recursively.
`Get-ChildItem` - Can be used to enumerate through file system objects.
`Get-ChildItem "C:\Program Files" -Recurse | Get-ACL | ?{$_.AccessToString -match "Everyone\sAllow\s\sModify"}` - Recursive searches in `C:\Program Files` and passes to `Get-ACL` then filter for the specific properties using the `AccessToString -match` field.

## Linux
`find` is a powerful command to locate various files with specific properties.
`find / -writable -type d 2>/dev/null` - Find all writable directories, filter out error output to `/dev/null`

# Unmounted Disks
For many systems, drives are automatically mounted at boot time. It's important to always look for unmounted drives and if possible check their mounting permissions.

## Windows
`mountvol` - used to list all drives that are currently mounted, as well as physically connected but unmounted drives.

## Linux
`mount` - list all the mounted filesystems.
`cat /etc/fstab` - Lists the drives that are automatically moutned at boot time.
`lsblk` - Shows available disks (Potentially some that are connected but not mounted)

# Device Drivers and Kernels Modules
Privilege escalation can often be accomplished by expoiting device drivers and kernel modules. It is important to compile a list of the currently loaded kernels modules and drivers.

## Windows
`driverquery /v /fo csv` - Lists installed drivers with Verbose output in CSV format.
Can be used in Powershell to pipe for easy filtering.
```
> powershell
PS > driverquery.exe /v /fo csv | ConvertFrom-CSV | Select-Object 'Display Name', 'Start Mode', Path
```

This outputs a list of installed drivers, however further enumeration is needed to find version numbers for each driver.
`Get-WmiObject` can be used to get the `Win32_PnPSignedDriver` WMI instance, which lists signature information for drivers. Piping output to `Select-Object` allows for enumeration of specific information. `Where-Object` can be used to filter for specific drivers.
`PS > Get-WmiObject Win32_PnPSignedDriver | Select-Object DeviceName, DriverVersion, Manufacturer | Where-Object {$_.DeviceName -like "*VMWARE*"}`

## Linux
`lsmod` - Lists the loaded kernel modules.
`/sbin/modinfo <MODULE NAME>` - Lists specific information on one module

# Binaries that AutoElevate
There exist certain possible shortcuts that may allow for quicker privilege escalation. These should always be checked for.

## Windows
Check the `AlwaysInstallELevated` registry setting. If set to `1` in `HKEY_CURRENT_USER` or `HKEY_LOCAL_MACHINE`, any user has permission to run Windows Installer packages with elevated privileges.

Query these fields using the following:
```
> reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\InstallerAlwaysInstallElevated    REG_DWORD    0x1

> reg query HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer
HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\InstallerAlwaysInstallElevated   REG_DWORD    0x1
```
If the setting is enabled, a custom craftred MSI file could be run to grant elevated privileges.

## Linux
It is important to check for SUID files. When these files are executed, they are run with the permissions of the file owner. If an SUID file is owned by root, any local use can run that binary with elevated privileges.

`find / -perm -u=s -type f 2>/dev/null` - Finds all files with SUID bit set and filters out error messages.