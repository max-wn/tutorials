---
title: Rsync
category: CLI
layout:
weight:
---

### Basic example

```bash
rsync -avz ./src /dest   # syncing folder src into dest:
rsync -avz ./src/ /dest  # syncing the content of src into dest:

# syncing the content of src into dest dir (example):
sudo rsync -aAXv --delete \
    ~/name_of_user/documents_1/ /media/name_of_user/USB_8GB/docs1_backup/
sudo rsync -aAXv --delete \
    ~/name_of_user/documents_2/ /media/name_of_user/WD_Passport/docs2_backup/

    -a  # archive (-rlptgoD)
    -A  # preserve ACLs (implies --perms)
    -X  # preserve extended attributes
    -v  # verbose
    -r  # recursive
    -P  # same as --partial --progress
    -x  # this tells rsync to avoid crossing a filesystem boundary when recursing
    -R  # use relative paths
    -S  # try to handle sparse files efficiently so they take up less space on the destination
    -z  #  compress file data during the transfer and decompress on destination
    -u  # skip files that are newer on the receiver
    -m  # prune empty directory chains from file-list
    --delete            # this tells rsync to delete extraneous files from the receiving side (ones that aren't on the sending side)
    --backup, -b        # make backups
    --backup-dir=DIR    # make backups into hierarchy based in DIR
    --exclude=/tmp/*    # exclude dir
    --exclude=".cache"  # exclude dir
```

### OSX

```bash
--exclude '.Trashes'
--exclude '.Spotlight-V100'
--exclude '.fseventsd'
```

### Transfer options

```bash
-z, --compress
-n, --dry-run
    --partial   # allows resuming of aborted syncs
    --bwlimit=RATE    # limit socket I/O bandwidth
```

### Display options

```bash
-q, --quiet
-v, --verbose
    --stats
-h, --human-readable
    --progress
-P                     # same as --partial --progress
```

### Skipping options

```bash
-u, --update     # skip files newer on dest
-c, --checksum   # skip based on checksum, not mod-time & size
```

### Backup options

```bash
-b, --backup           # backup with suffix
    --suffix=SUFFIX    # default ~ without --backup-dir
    --backup-dir=DIR
```

### Include options

```bash
--exclude=PATTERN
--include=PATTERN
```

```bash
--exclude-from=FILE
--include-from=FILE
--files-from=FILE    # read list of filenames from FILE
```

```bash
-C, --cvs-exclude    # exclude from local/global .cvsignore
```

### Archive options

```bash
-a, --archive    # archive (-rlptgoD)
```

```bash
-r, --recursive
-l, --links      # copy symlinks as links
-p, --perms      # preserve permissions
-t, --times      # preserve times
-g, --group      # preserve group
-o, --owner      # preserve owner
-D               # same as --devices --specials
```

```bash
--delete         # Delete extra files
```

---

THE END
