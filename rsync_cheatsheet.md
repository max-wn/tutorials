# rsynk cheatsheet

---

`sudo rsync -aAXv --delete ~/nils/documents_wd/ /media/nils/PROGS_8GB/DOCS_BACKUP/`
`sudo rsync -aAXv --delete ~/nils/documents_wd/ /media/nils/WD_Passport/LINUX_BACKUP/`

   `-a` archive (-rlptgoD)
   `-A` preserve ACLs (implies --perms)
   `-X` preserve extended attributes
   `-v` verbose
   `-r` recursive
   `-P` same as --partial --progress
   `-x` this tells rsync to avoid crossing a filesystem boundary when recursing
   `-R` use relative paths
   `-S` try to handle sparse files efficiently so they take up less space on the destination
   `--delete`  this  tells  rsync  to  delete  extraneous  files from the receiving side (ones that aren't on the sending side)
   `--backup`
   `--backup-dir`
   `-z` compress file data during the transfer and decompress on destination
   `--exclude=/tmp/*`
   `--exclude=".cache"`
   `-u`
   `-m`

---

THE END
