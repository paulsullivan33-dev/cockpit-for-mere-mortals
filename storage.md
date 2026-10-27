# Storage

After this chapter you will be able to read the Storage page
like a map — what disks the machine has, how they are divided,
where they are mounted — and you will know exactly which
buttons on that page can destroy data, because several can.

Cockpit's Storage page is drawn by the `cockpit-storaged`
package and works through **udisks2**, the system's disk
service. Everything it shows is real, live, and the same
information `lsblk` and `df` would give you — arranged so the
structure is visible instead of inferred.

## Reading the page

At the top, a graph of disk activity and a summary of
filesystems by how full they are — the full ones first, which
is the correct order for worry. Below, the machine's storage,
organised the way the hardware is:

- **Drives** — the physical (or virtual) disks, with model,
  size, temperature where the disk reports it, and an overall
  health word. A disk that has started to fail announces itself
  here before it announces itself at 3 a.m.
- **Filesystems** — for each formatted volume: where it is
  **mounted** (the directory it appears at, such as `/` or
  `/home`), its size, and how much is free. *Mounting* is
  attaching a filesystem at a directory so its files appear
  there; an unmounted filesystem is present but invisible to
  ordinary programs.
- Where the machine uses them: **LVM** volume groups and
  logical volumes (a layer that lets several disks be pooled
  and divided up flexibly), **RAID** arrays (several disks
  acting as one, with redundancy), and **NFS mounts** (folders
  belonging to another machine, attached over the network).

Click any item for its detail page: usage over time, the exact
device name (such as `/dev/sda2`), its UUID (the filesystem's
own serial number, which is how mount configuration refers to
it), and the actions available for it.

## The careful chapter: things you can do, in rising order of danger

**Mount and unmount.** Safe in the ordinary case: unmounting a
filesystem makes its files vanish until it is mounted again.
Unmounting the wrong one at the wrong time — especially the
root filesystem's friends — can still ruin an afternoon, so
read the mount point before you click.

**Format a partition or disk.** This is the point of no return,
and Cockpit labels it honestly: formatting writes a fresh, empty
filesystem over the old one's identity. Everything that was on
it stops being reachable by ordinary means. The format dialog
makes you choose a filesystem type (ext4 and XFS are the usual
Linux choices) and type to confirm. Rule of the page: before
formatting anything, state out loud which disk the machine
boots from, and confirm the size and model on screen match the
disk you mean to erase. People rarely format the wrong disk
slowly; they do it confidently and fast.

**Create and delete partitions, LVM volumes, and RAID arrays.**
All possible from this page: carving a disk into partitions,
pooling disks into a volume group, building a mirrored array.
Each is a real administrative act done with the system's own
tools underneath (the same operations `parted`, `lvm`, and
`mdadm` perform), logged as yours. Create on empty disks;
delete only what you have already read twice.

**Check and repair a filesystem.** An unmounted filesystem can
be checked from its page — the web version of `fsck`. Useful,
safe, and occasionally the whole reason for the visit.

The honest summary of this chapter: the Storage page is the
most dangerous page in Cockpit, because it is the one where a
click can be unrecoverable. The console does not add risk that
the command line lacks — `mkfs` is equally final — but it puts
the whole disk inventory, and the erase buttons, one screen
apart. Work here slowly, on machines whose backups you believe
in, and let the page's confirmations ask their questions.
