# Introduction
This document provides an alternative method for mounting an external SD card to the `printer_data` directory.

## usb-flash-watch
Run the following command and copy the content into usb-flash-watch file. 
```
sudo nano /usr/local/sbin/usb-flash-watch
```
```
#!/usr/bin/env bash
set -euo pipefail

mnt="/mnt/usb_flash"

mkdir -p "$mnt"

mounted_src="$(findmnt -rn -o SOURCE "$mnt" 2>/dev/null || true)"
target=""

# Prioritize partitions such as /dev/sda1 and /dev/sdb1.
for dev in /dev/sd*[0-9]; do
  [ -b "$dev" ] || continue
  if blkid -o value -s TYPE "$dev" >/dev/null 2>&1; then
    target="$dev"
    break
  fi
done

# Scenarios where a compatible file system is created directly on a whole disk, such as /dev/sda.
if [ -z "$target" ]; then
  for dev in /dev/sd?; do
    [ -b "$dev" ] || continue
    if blkid -o value -s TYPE "$dev" >/dev/null 2>&1; then
      target="$dev"
      break
    fi
  done
fi

if [ -n "$target" ]; then
  if [ "$mounted_src" = "$target" ]; then
    exit 0
  fi

  if [ -n "$mounted_src" ]; then
    umount "$mnt" || umount -l "$mnt" || true
  fi

  fstype="$(blkid -o value -s TYPE "$target" 2>/dev/null || true)"
  [ -n "$fstype" ] || exit 0

  opts="rw,nosuid,nodev,noatime"

  case "$fstype" in
    vfat|exfat|ntfs)
      opts="$opts,uid=1000,gid=1000,umask=022"
      ;;
  esac

  mount -t "$fstype" -o "$opts" "$target" "$mnt"
  exit 0
fi

# Unmount when there are no /dev/sd* filesystems.
if [ -n "$mounted_src" ]; then
  umount "$mnt" || umount -l "$mnt" || true
fi

```

make the usb-flash-watch file executable.
```
sudo chmod +x /usr/local/sbin/usb-flash-watch
```


## usb-flash-watch.service
Run the following command and copy the content into usb-flash-watch.service file. 
```
sudo nano /etc/systemd/system/usb-flash-watch.service
```

```
[Unit]
Description=Watch and mount sd* USB storage to /mnt/usb_flash

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/usb-flash-watch

sudo nano /etc/systemd/system/usb-flash-watch.timer
[Unit]
Description=Run USB flash watcher periodically

[Timer]
OnBootSec=5s
OnUnitActiveSec=3s
AccuracySec=1s
Unit=usb-flash-watch.service

[Install]
WantedBy=timers.target

```

Use the following commands to start the service and verify the results; finally, you can access the inserted SD card in the `~/printer_data/gcodes/SD` directory.
```
sudo systemctl daemon-reload
sudo systemctl enable --now usb-flash-watch.timer
sudo systemctl start usb-flash-watch.service
findmnt /mnt/usb_flash  # Check if the SD card is mounted at /mnt/usb_flash.
lsblk -f  # List information about all block devices (such as hard drives and USB drives).
systemctl status usb-flash-watch.timer
sudo ln -s /mnt/usb_flash /home/linaro/printer_data/gcodes/
```