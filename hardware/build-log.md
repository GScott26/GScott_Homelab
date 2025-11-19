# PROXMOX VE SERVER BUILD LOG

## BUILD INFORMATION
- **Start:** Nov. 22, time TBD

---

> **TL;DR:**

---

## PRE-BUILD CHECKLIST

### PARTS INVENTORY

**Verified and ready:**

- **CPU:** Intel Core i7-10700F (8C/16T)
  - Source: Previous desktop build
  - Condition: Working, no issues
  
- **Motherboard:** MSI Z490-A Pro
  - Source: Same desktop build
  - Verified: Supports VT-x/VT-d for virtualization [1]
  
- **CPU Cooler:** Intel E97379-003 (Stock LGA1200/115x) [2]
  - Source: Spare parts from older build
  - Condition: Tested, fan spins freely, minimal dust
  
- **RAM:** Corsair Vengeance 16GB (2x8GB DDR4)
  - Source: Friend's old AM4 build
  - Speed: Will run at 3200MHz with XMP enabled [3]
  - Previous owner: No stability issues reported
  
- **Storage:** Western Digital 500GB 2.5" SATA III HDD
  - Source: Old laptop (~2018)
  - Concerns: Age, speed, reliability (see notes below) [4]
  
- **PSU:** 500W 80+ Gold (model TBD - will check during build)
  - Source: Friend donation
  - Verified: Powers on, all cables present
  
- **Case:** ATX mid-tower (model TBD)
  - Source: Friend donation
  - Verified: All expansion slots, drive bays intact
  
- **GPU:** NVIDIA GeForce GTX 650 (TEMPORARY)
  - Purpose: Initial setup only
  - Will remove: After web interface verified working [5]

#### Build Notes
**[1] Motherboard - Virtualization Support:**
Z490-A PRO verified to support hardware virtualization (VT-x/VT-d) with the i7-10700F. Will enable during BIOS configuration.

**[2] CPU Cooler - Stock vs Aftermarket:**
Intel stock cooler verified working. Should handle server workload (no overclocking). Will monitor temps and upgrade to aftermarket if exceeding 75°C under load.

**[3] RAM - XMP Configuration:**
Originally planned to run at JEDEC speeds (2666MHz) for stability. However, this kit ran stable at 3200MHz for previous owner. Will enable XMP for better VM performance. Can always disable if instability occurs.

**[4] Storage - HDD Concerns & Upgrade Path:**
- **Age/Health:** Unknown hours, from laptop manufactured around 2018 - will run SMART test in shell once Proxmox VE is installed
- **Speed:** ~100MB/s read/write vs SSD's 500MB/s - expect slower VM performance
- **Reliability:** Single point of failure - will implement backups immediately
- **Upgrade Path:** Planning to add m.2 SATA or NVMe SSD (500GB-1TB) within 1-2 months
- **Budget:** ideally ~80-100 for new NVMe SSD, but potentially ~$30-50 for used SATA III / m.2 SATA SSD if necessary

  **Decision:** Acceptable for early projects, but will migrate critical VMs (Minecraft server, Pi-hole, etc.) to SSD when purchased.

**[5] GPU - Temporary Installation:**
- **Used for:** BIOS config, Proxmox installation, network verification
- **Removal timeline:** After confirming web interface access (192.168.4.10:8006)
- **BIOS setting verified:** Will enable proper UEFI settings once server POSTs
- **Power savings:** ~55W reduction compared to leaving GPU installed
- **Kept accessible:** Will store GPU nearby for troubleshooting if needed

---

## POST-BUILD TESTING PLAN

### Initial Power-On Tests
1. **Visual inspection** - Check all connections, no loose screws
2. **First POST** - Should see BIOS screen after a few moments, verify with boot debug LEDs
3. **BIOS verification** - Check all components detected (CPU, RAM, storage)
4. **Temperature check** - CPU should be <40°C idle in BIOS

### Configuration
1. **BIOS settings** - Enable VT-x, VT-d, XMP, configure boot options
2. **Stability test** - Let sit in BIOS for 10 minutes, check temps stay stable
3. **USB boot/PVE installation** - Boot from Proxmox installer USB
4. **Network test** - Verify ethernet connection during install using port 'link' and 'activity' LEDs

### Post-Installation Tests
1. **Web interface** - Access from laptop at 192.168.4.10:8006
2. **Internet connectivity** - `ping 8.8.8.8` and `ping google.com` from PVE shell
3. **HDD Health** - Run `smartctl -a /dev/sda` command from PVE shell
     - Possibly create a bash script to run SMART test daily (or see if such a utility is pre-installed in PVE)
5. **Reboot test** - Reboot server, verify it comes back up
6. **Enable and test headless boot** - Once appropriate settings are enabled, ensure server powers on and boots into PVE by using web interface.

### Success Criteria
- [X] System POSTs successfully
- [X] All components recognized
- [X] Proxmox installed and accessible via web interface
- [X] Network connectivity confirmed
- [X] Temps <75°C under load
- [X] No unusual noises (fan grinding, coil whine)
