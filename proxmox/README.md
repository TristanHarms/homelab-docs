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

## LXC shared igpu passhtrough hookup script

`/var/lib/vz/snippets/lxc-igpu.sh` (`chmod +x`)
```bash
#!/usr/bin/perl

use strict;
use warnings;

# Retrieve the container ID and the execution phase from arguments
my $vmid = shift;
my $phase = shift;

# Ensure we are in the 'pre-start' phase
if ($phase eq 'pre-start') {

    # The location where Proxmox generates the temporary LXC config
    my $config_file = "/var/lib/lxc/$vmid/config";

    # Open the config file in append mode
    open(my $fh, '>>', $config_file) or die "Could not open config file: $!";

    print "Hookscript: Adding GPU/DRM passthrough config for VM $vmid\n";

    # Append the requested configuration lines
    # Note: We ensure standard LXC 'key = value' syntax
    print $fh "lxc.cgroup2.devices.allow = c 226:* rwm\n";
    print $fh "lxc.mount.entry = /dev/dri dev/dri none bind,optional,create=dir\n";

    close($fh);
}

exit 0;
```
