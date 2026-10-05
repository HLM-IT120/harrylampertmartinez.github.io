## Connecting a Proxmox to OpenMediaVault

**The Objective**
This week, my goal was to expand the storage capacity of my Proxmox home lab. To do this, I repurposed a laptop connected to a 4-bay Direct-Attached Storage (DAS) enclosure by installing OpenMediaVault (OMV) 8. The objective was to configure this setup as a Network Attached Storage (NAS) node and connect it to my Proxmox server to handle container backups and media files. 

**The Hurdle**
I immediately hit a wall on two fronts: physical hardware and network permissions. 

First, my Proxmox server refused to boot properly. The boot sequence would hang indefinitely. Once I got past the hardware issue and finally attempted to connect Proxmox to the new OMV NAS over the network via NFS, Proxmox threw a strict **"Permission denied (500)"** error. Additionally, I discovered that if the OMV laptop wasn't powered on and ready, the Proxmox server would hang during its boot sequence because it was waiting for a network share that didn't exist.

**The Solution**
This required a multi-step troubleshooting approach:

*   **The Hardware Boot Fix:** I opened up the server and disconnected a ribbon-to-SATA cable I had recently installed. The boot sequence immediately went back to normal. **Why did this work?** When adding new drives or niche SATA adapters, a motherboard's BIOS will often change the boot order automatically or hang up on the udev queue while trying to initialize a faulty hardware connection. Removing the problematic cable stopped the system from hanging on initialization. 
*   **The Permission Denied (500) Fix:** In the OpenMediaVault dashboard, I navigated to the NFS share settings and added `no_root_squash` to the extra options field. **Why did this work?** By default, NFS has a security feature that "squashes" (downgrades) requests from a root user into a standard, unprivileged anonymous user. Because Proxmox requires root-level privileges to mount the remote file system, adding `no_root_squash` tells the NAS to trust the Proxmox root user.
*   **The Network Hang Fix:** To fix the issue where Proxmox would hang if OMV was offline, I had to boot Proxmox into recovery mode, remount the file system as read-write, and edit the Proxmox storage configuration file. 

Here is the command I used to open the configuration file and safely comment out the missing network share so the server could boot:

```bash
# Open the storage configuration file in the Nano text editor
nano /etc/pve/storage.cfg

# I then added a # symbol in front of my NFS share lines to temporarily disable them
# nfs: omv-backup
#    export /export/backup
#    server 192.168.1.100
#    content backup
