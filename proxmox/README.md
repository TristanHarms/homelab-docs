# Proxmox

## TODO

- [ ] [Revert zfs arc changes to default]([url](https://gemini.google.com/share/a6932376b5fc))

## Loading video drivers on the Intel Ultra 9 285HX

```bash
echo "options i915 modeset=1" > /etc/modprobe.d/i915.conf
update-initramfs -u -k all
reboot
ls -la /dev/dri
```

## LXC Debian shared igpu passhtrough pre-start hookup script

`/var/lib/vz/snippets/lxc-debian-igpu.sh` (`chmod +x`)

```sh
#!/bin/bash

# Get the Container ID and Phase from arguments
vmid="$1"
phase="$2"

# Only run during the 'pre-start' phase
if [[ "$phase" == "pre-start" ]]; then
    echo "Hookscript: Adding GPU passthrough config for VM $vmid"

    # Define the config path
    CONFIG="/var/lib/lxc/$vmid/config"

    # Check if the config file actually exists before writing
    if [ -f "$CONFIG" ]; then
        echo "lxc.cgroup2.devices.allow = c 226:* rwm" >> "$CONFIG"
        echo "lxc.mount.entry = /dev/dri dev/dri none bind,optional,create=dir" >> "$CONFIG"
    else
        echo "Error: Config file not found at $CONFIG" >&2
        exit 1
    fi
fi

exit 0
```

## LXC Alpine Ansible Prep hookup script

`/var/lib/vz/snippets/lxc-alpine-ansible-prep.sh` (`chmod +x`)

```sh
#!/bin/bash

# Get container ID from environment
vmid=$1

# Wait for container to be fully started
sleep 5

# Install and start SSH in the Alpine container
pct exec $vmid -- sh -c '
    # Update package index
    apk update

    # Install Python, needed for Ansible
    apk add openssh python3

    # Install OpenSSH, needed for Ansible
    apk add openssh

    # Generate SSH host keys if they don'\''t exist
    ssh-keygen -A

    # Enable and start SSH service
    rc-update add sshd default
    rc-service sshd start
'

exit 0
```
