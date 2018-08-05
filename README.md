trinkup - TRivial INcremental bacKUP script
===========================================

> Уж 200 раз твердили Сене:
> Хардлинк спасет от удаленья!
> А кто создать его поможет?
> Crontab и man, тупая рожа!
>
> (c) linux.org.ru, no-dashi

Trinkup is intended to be the simplest possible tool for incremental backups.
It holds the specified number of snapshots for the specified rsync source
(which could be just a local filesystem path or `hostname:/path` for
rsync-over-SSH). Snapshots are named by the current datetime (in ISO format)
and are stored in the specified base directory, which should be different for
each source.

Each snapshot is a directory. It is possible to `cd` to the snapshot directory
and work with the files in it, though it is recommended to make a separate copy
first. Unchanged files are copied as a hardlinks between snapshots.

Usage
-----

```
trinkup <rsync_source> <base_directory> <number_of_backups> [rsync_args]
```

Crontab example of rsync-over-SSH daily backups holding the last 7 days:

```
5 0 * * * /usr/local/bin/trinkup example.com:/home /backups/example.com/home 7
```

Why not just rsync?
-------------------

Really, rsync could create hardlinks by itself. But it is not useful for
backups since hardlinks only work within the single filesystem. Trinkup creates
hardlinks from the latest backup directory, not from the source, so it doesn't
need to cross the filesystem boundary.
