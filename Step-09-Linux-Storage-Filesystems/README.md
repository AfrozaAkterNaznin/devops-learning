# Linux Storage & Filesystems

---

# Learning Objectives

After completing this step, you will be able to:

- Understand Linux storage architecture.
- Identify disks and partitions.
- Differentiate physical disks, partitions, and filesystems.
- Inspect block devices.
- Monitor filesystem usage.
- Analyze directory size.
- Understand mounting and unmounting.
- Read mount information.
- Identify filesystem types.
- Inspect partition tables.
- Use professional Linux storage inspection commands.

---

# Storage Workflow

```text
Physical Disk
      │
      ▼
Partition Table (GPT / MBR)
      │
      ▼
Partitions
      │
      ▼
Filesystem (ext4, xfs, vfat ...)
      │
      ▼
Mount Point
      │
      ▼
Accessible Directory
```

---

# Linux Storage Architecture

```text
Hard Disk
(/dev/sda)
      │
      ├── sda1
      │
      └── sda2
             │
             ▼
         ext4 Filesystem
             │
             ▼
          Mounted on /
```

---

# Storage Inspection Workflow

```text
Disk
 │
 ▼
lsblk
 │
 ▼
Filesystem
 │
 ▼
blkid
 │
 ▼
Mounted?
 │
 ▼
findmnt
 │
 ▼
Space Usage
 │
 ├── df
 │
 └── du
```

---

# Commands Covered

| Section | Command |
|----------|----------|
| Disk Information | lsblk |
| Filesystem Information | lsblk -f |
| Custom Output | lsblk -o |
| Filesystem UUID | blkid |
| Filesystem Usage | df |
| Directory Usage | du |
| Mount Information | mount |
| Mounted Filesystems | findmnt |
| Filesystem Types | cat /proc/filesystems |
| Mount / Unmount | mount, umount |
| Partition Information | fdisk |
| Partition Table | parted |

---

# Command Variations

## lsblk

| Command | Purpose |
|----------|----------|
| lsblk | Show disks and partitions |
| lsblk -f | Show filesystem information |
| lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINT | Custom columns |
| lsblk -a | Show all block devices |

---

## df

| Command | Purpose |
|----------|----------|
| df -h | Human-readable disk usage |
| df -Th | Show filesystem type |
| df -i | Show inode usage |
| df / | Show root filesystem usage |

---

## du

| Command | Purpose |
|----------|----------|
| du -sh | Current directory size |
| du -sh ~ | Home directory size |
| du -h --max-depth=1 | Top-level directory sizes |
| du -ah | Include files |
| du -ah \| sort -hr | Sort by size |

---

## findmnt

| Command | Purpose |
|----------|----------|
| findmnt | Show mounted filesystems |
| findmnt / | Root mount |
| findmnt /home | Home mount |
| findmnt /run/media/... | Specific mount |

---

## fdisk

| Command | Purpose |
|----------|----------|
| sudo fdisk -l | List partitions |

---

## parted

| Command | Purpose |
|----------|----------|
| sudo parted -l | Show partition tables |

---

# Important Flags

| Flag | Meaning |
|------|----------|
| -h | Human-readable |
| -T | Show filesystem type |
| -i | Show inode usage |
| -a | Show all |
| -f | Filesystem information |
| -o | Select output columns |
| -l | List |
| --max-depth | Limit recursion depth |

---

# What to Observe

| Step | Observe |
|------|----------|
| **9.1** | Disk names (`sda`), partitions (`sda1`, `sda2`), filesystem (`ext4`), mount point (`/`), UUID, available space |
| **9.2** | Total disk size, used space, available space, directory sizes, largest directories |
| **9.3** | Mounted filesystems, mount points, filesystem types, mount hierarchy |
| **9.4** | Root filesystem mount, current mounted location, VirtualBox ISO mount |
| **9.5** | Home directory size, largest folders, inode usage, overall filesystem usage |
| **9.6** | Mount operation, mounted tmpfs, accessibility after mount, disappearance after unmount |
| **9.7** | Disk model, partition table (GPT), partition layout, filesystem type, partition information |

---

# Output Interpretation

| Output | Meaning |
|---------|---------|
| `/dev/sda` | Main virtual hard disk |
| `sda1` | BIOS boot partition |
| `sda2` | Linux partition |
| `ext4` | Linux filesystem |
| `/` | Root mount point |
| `tmpfs` | RAM-based temporary filesystem |
| `iso9660` | CD/DVD filesystem |
| `100%` on squashfs | Normal (read-only Snap package) |

---

# Professional Inspection Order

```text
1. lsblk
        ↓
2. blkid
        ↓
3. df
        ↓
4. du
        ↓
5. findmnt
        ↓
6. fdisk
        ↓
7. parted
```

---

# Best Practices

- Always inspect disks before modifying partitions.
- Never edit partition tables without backups.
- Use `df` for filesystem usage.
- Use `du` for directory usage.
- Verify mount points before copying files.
- Prefer UUID over device names in permanent configurations.
- Understand the difference between disk usage and directory usage.
- Check filesystem type before formatting.

# Block Devices

A **block device** is a storage device that stores data in fixed-size blocks.

Examples:

- HDD
- SSD
- NVMe SSD
- USB Drive
- Virtual Disk

Linux represents block devices inside the `/dev` directory.

Example:

```text
/dev/sda
/dev/sdb
/dev/nvme0n1
```

---

# Disk vs Partition vs Filesystem

| Component | Description | Example |
|-----------|-------------|---------|
| Disk | Physical or virtual storage device | `/dev/sda` |
| Partition | Logical division of a disk | `/dev/sda2` |
| Filesystem | Organizes files on a partition | `ext4` |
| Mount Point | Directory where filesystem becomes accessible | `/` |

---

# Disk Naming Convention

| Device | Meaning |
|---------|---------|
| sda | First SATA/SCSI disk |
| sdb | Second disk |
| sdc | Third disk |
| nvme0n1 | First NVMe SSD |
| loop0 | Loopback device (Snap package) |
| sr0 | CD/DVD-ROM device |

---

# Filesystem Types

| Filesystem | Platform | Description |
|------------|----------|-------------|
| ext4 | Linux | Default Linux filesystem |
| xfs | Linux | High-performance filesystem |
| btrfs | Linux | Modern copy-on-write filesystem |
| vfat | Windows/Linux | FAT32 compatible |
| exfat | Windows/Linux | Flash drives & SD cards |
| ntfs | Windows | Windows filesystem |
| iso9660 | CD/DVD | Optical media filesystem |
| squashfs | Linux | Compressed read-only filesystem |
| tmpfs | Linux | RAM-based temporary filesystem |

---

# Mounting Process

```text
Storage Device
       │
       ▼
Filesystem
       │
       ▼
Mount Command
       │
       ▼
Mount Point
       │
       ▼
Accessible Files
```

---

# Mount vs Unmount

| Mount | Unmount |
|--------|----------|
| Connects a filesystem | Disconnects a filesystem |
| Makes files accessible | Removes access |
| Uses `mount` | Uses `umount` |
| Creates an active mount point | Keeps only the empty directory |

---

# Common Mount Points

| Mount Point | Purpose |
|-------------|---------|
| `/` | Root filesystem |
| `/home` | User files |
| `/boot` | Boot files |
| `/mnt` | Temporary manual mounts |
| `/media` | USB/CD/DVD mounts |
| `/run/media` | Desktop auto-mounted devices |
| `/tmp` | Temporary files |

---

# Understanding lsblk Output

| Column | Meaning |
|---------|---------|
| NAME | Device name |
| SIZE | Device size |
| TYPE | Disk, partition, loop, ROM |
| FSTYPE | Filesystem type |
| MOUNTPOINT | Mounted directory |

---

# Understanding df Output

| Column | Meaning |
|---------|---------|
| Filesystem | Mounted filesystem |
| Size | Total size |
| Used | Used storage |
| Avail | Free storage |
| Use% | Usage percentage |
| Mounted on | Mount location |

---

# Understanding du Output

| Output | Meaning |
|---------|---------|
| `du -sh` | Total directory size |
| `du -ah` | Include individual files |
| `--max-depth=1` | Only first-level directories |
| `sort -hr` | Largest items first |

---

# Understanding findmnt Output

| Column | Meaning |
|---------|---------|
| TARGET | Mount point |
| SOURCE | Device |
| FSTYPE | Filesystem type |
| OPTIONS | Mount options |

---

# Understanding blkid Output

| Field | Meaning |
|--------|---------|
| UUID | Unique filesystem identifier |
| TYPE | Filesystem type |
| LABEL | Filesystem label |
| PARTUUID | Unique partition identifier |

---

# Understanding fdisk Output

| Field | Meaning |
|--------|---------|
| Disk | Storage device |
| Disklabel | GPT or MBR |
| Sector Size | Logical & physical block size |
| Device | Partition |
| Size | Partition size |
| Type | Partition purpose |

---

# Understanding parted Output

| Field | Meaning |
|--------|---------|
| Model | Disk model |
| Disk | Device |
| Partition Table | GPT / MBR |
| Number | Partition number |
| Start | Start position |
| End | End position |
| File System | Filesystem |
| Flags | Special partition flags |

---

# Storage Inspection Commands Summary

| Task | Command |
|------|---------|
| Show disks | `lsblk` |
| Show filesystems | `lsblk -f` |
| Show UUID | `blkid` |
| Show mounted filesystems | `findmnt` |
| Show disk usage | `df -h` |
| Show directory usage | `du -sh` |
| Show partition table | `sudo fdisk -l` |
| Show GPT information | `sudo parted -l` |
| Mount filesystem | `mount` |
| Unmount filesystem | `umount` |

---

# Real Environment Summary

| Property | Value |
|-----------|-------|
| Disk | `/dev/sda` |
| Disk Size | `60.1 GB` |
| Partition Table | GPT |
| Root Partition | `/dev/sda2` |
| Filesystem | ext4 |
| Root Mount | `/` |
| Optical Device | `/dev/sr0` |
| Optical Filesystem | iso9660 |
| Snap Packages | squashfs |
| Temporary Filesystem | tmpfs |

---

# Best Practices

- Verify the correct disk before performing storage operations.
- Use `lsblk` before partitioning or formatting.
- Check available space with `df`.
- Use `du` to identify large directories.
- Prefer UUID for persistent mounts.
- Unmount a filesystem before removing external storage.
- Never modify partitions without a backup.
- Confirm the filesystem type before mounting.



# Storage Inspection Checklist

Before making any storage-related changes on a Linux server, a Linux Administrator or DevOps Engineer typically follows this checklist.

| Step | Command | Purpose |
|------|----------|---------|
| 1 | `lsblk` | Identify disks and partitions |
| 2 | `lsblk -f` | Identify filesystem types |
| 3 | `blkid` | Check UUID and labels |
| 4 | `findmnt` | Verify mounted filesystems |
| 5 | `df -h` | Check free disk space |
| 6 | `du -sh` | Find large directories |
| 7 | `sudo fdisk -l` | Inspect partition layout |
| 8 | `sudo parted -l` | Verify partition table |

---

# Real Lab Summary

During this lab, the following storage components were inspected.

| Component | Result |
|-----------|--------|
| Main Disk | `/dev/sda` |
| Root Partition | `/dev/sda2` |
| Filesystem | `ext4` |
| Partition Table | `GPT` |
| Mount Point | `/` |
| Optical Device | `/dev/sr0` |
| Optical Filesystem | `iso9660` |
| Snap Images | `squashfs` |
| RAM Filesystem | `tmpfs` |

---

# Commands Practiced

```bash
lsblk

lsblk -f

lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINT

blkid

df -h

df -Th

df -i

du -sh

du -sh ~

du -h --max-depth=1

du -ah | sort -hr | head -20

mount

findmnt

findmnt /

findmnt /run/media/$USER/VBox_GAs_7.2.12

cat /proc/filesystems

sudo mount -t tmpfs -o size=100M tmpfs ~/mount-demo

sudo umount ~/mount-demo

sudo fdisk -l

sudo parted -l
```

---

# Key Differences

## Disk vs Partition

| Disk | Partition |
|------|-----------|
| Physical or virtual storage device | Logical section of a disk |
| Example: `/dev/sda` | Example: `/dev/sda2` |

---

## df vs du

| df | du |
|----|----|
| Filesystem usage | Directory/File usage |
| Shows total disk usage | Shows folder size |
| Used for storage monitoring | Used for locating large directories |

---

## lsblk vs blkid

| lsblk | blkid |
|--------|-------|
| Shows block device tree | Shows UUID and filesystem metadata |
| Better for storage layout | Better for filesystem identification |

---

## mount vs findmnt

| mount | findmnt |
|--------|----------|
| Shows all mounted filesystems | Shows mount tree in a structured format |
| Older command | Modern and easier to read |

---

## fdisk vs parted

| fdisk | parted |
|--------|---------|
| Traditional partition tool | Modern partition management tool |
| Good for MBR & GPT | Better GPT support |
| Widely used | Preferred for modern disks |

---

# Common Filesystem Types

| Filesystem | Used For |
|------------|----------|
| ext4 | Linux |
| xfs | Enterprise Linux |
| btrfs | Modern Linux |
| vfat | USB / FAT32 |
| exfat | Flash drives |
| ntfs | Windows |
| iso9660 | CD/DVD |
| squashfs | Snap packages |
| tmpfs | RAM storage |

---

# Common Storage Commands for DevOps

| Purpose | Command |
|----------|----------|
| Inspect disks | `lsblk` |
| Check filesystem | `lsblk -f` |
| View UUID | `blkid` |
| Disk usage | `df -h` |
| Directory usage | `du -sh` |
| Mounted filesystems | `findmnt` |
| Mount filesystem | `mount` |
| Unmount filesystem | `umount` |
| Inspect partitions | `fdisk -l` |
| Check GPT | `parted -l` |

---

# Professional Workflow

```text
Identify Disk
      │
      ▼
Inspect Partitions
      │
      ▼
Identify Filesystem
      │
      ▼
Verify Mount Point
      │
      ▼
Check Free Space
      │
      ▼
Find Large Directories
      │
      ▼
Verify Partition Layout
      │
      ▼
Safe Storage Administration
```

---

# Best Practices

- Always verify the correct disk before performing storage operations.
- Never format or partition the wrong device.
- Use `UUID` instead of device names in persistent configurations.
- Check available disk space before installing software.
- Use `du` regularly to identify large directories.
- Unmount removable storage before disconnecting it.
- Verify the filesystem type before mounting.
- Keep sufficient free disk space to avoid system issues.
- Review partition layouts before resizing disks.
- Perform backups before modifying partitions.

---

# Conclusion
In this step, I learned how Linux organizes storage devices, partitions, filesystems, and mount points. I practiced inspecting disks, monitoring storage usage, analyzing directory sizes, mounting and unmounting filesystems, and verifying partition layouts using professional Linux administration commands. These skills form the foundation for Linux System Administration, Backend Engineering, DevOps, Cloud Infrastructure, Docker, Kubernetes, and Production Server Management.
