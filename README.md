# pve-autosnap
 Proxmox automatic snapshot tool 

## Usage

```bash
Usage:    pve-autosnap --mode [--storage=STORAGENAME] [--leave=NUMBER] [vmid @pool ...]

Example:  pve-autosnap --weekly --storage=ceph --leave=2
     or:  pve-autosnap --yearly --storage=pve-data
     or:  pve-autosnap -d -s stor -l 4

Arguments:
    -d, --daily                 Run this script in mode for daily autosnapshots
    -w, --weekly                Run this script in mode for weekly autosnapshots
    -m, --monthly               Run this script in mode for monthly autosnapshots
    -y, --yearly                Run this script in mode for yearly autosnapshots
    -s, --storage=STORAGENAME   Specify the storage name for which will be used auto snapshots
                                (0 or not specified will enable for all
    -l, --leave=NUMBER          Specify the number of snapshots which should will leave, anything longer will be removed
                                (0 or not specified will disable removing snapshots)

    vmid                        List of vm ids to be snapshotted (empty = all)
    @pool                       Name of vm pool to be snapshotted (case sensitive!)
```

## Enable pve-autosnap

Just add your rules to crontab, example:
```bash
VMIDS="100 102 105 110"

crontab -l > crontab.txt

cat >> crontab.txt << EOF
# Daily snapshot
0 3 * * 1-6 /bin/pve-autosnap --daily --leave=3 $VMIDS
# Weekly snapshot
0 3 * * 7 /bin/pve-autosnap --weekly --leave=3 $VMIDS
# Monthly snapshot
0 3 1 * * /bin/pve-autosnap --monthly --leave=4 $VMIDS
EOF

crontab crontab.txt
rm crontab.txt
```
