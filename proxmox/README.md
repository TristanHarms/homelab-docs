# Proxmox

## Loading video drivers on the Intel Ultra 9 285HX

```bash
echo "options i915 modeset=1" > /etc/modprobe.d/i915.conf
update-initramfs -u -k all
reboot
ls -la /dev/dri
```