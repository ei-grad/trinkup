trinkup - TRivial INcremental bacKUP script
===========================================

```
    Уж 200 раз твердили Сене:       A hundred times to Sam they've said,
    Хардлинк спасет от удаленья!    Hard links save files from being dead.
    А кто создать его поможет?      But who will aid in their creation?
    Crontab и man, тупая рожа!      Crontab and man, strong foundation!

    (c) linux.org.ru, no-dashi
```

Introduction
------------

Trinkup is designed to be the simplest possible tool for creating incremental backups. It maintains the specified number of snapshots for a given rsync source, which can either be a local filesystem path or a remote path using rsync-over-SSH (e.g., `hostname:/path`).

Snapshots are named using the current date and time in ISO format and stored in a specified base directory. Each source should have its own unique base directory. Each snapshot is a directory containing the backed-up files. To save space, unchanged files are stored as hard links between snapshots.

Prerequisites
-------------

To use trinkup, you need:

- Bash shell
- rsync

Installation
------------

1. Download the `trinkup` script.
2. Place the script in a convenient location, such as `/usr/local/bin/`.
3. Make the script executable by running `chmod +x /path/to/trinkup`.

Configuration
-------------

If you plan to use rsync-over-SSH, make sure to set up SSH keys for passwordless authentication. Consult the [official SSH documentation](https://www.ssh.com/ssh/keygen/) for more information.

Usage
-----

```
trinkup <rsync_source> <base_directory> <number_of_backups> [rsync_args]
```

Examples
--------

1. Daily local backup of the `/home` directory, retaining the last 7 days of backups:

```
5 0 * * * /usr/local/bin/trinkup /home /backups/home 7
```

2. Daily rsync-over-SSH backup, retaining the last 7 days of backups:

```
5 0 * * * /usr/local/bin/trinkup example.com:/home /backups/example.com/home 7
```

Troubleshooting
---------------

If you encounter issues, check the following:

- Ensure that rsync is installed and in your system's PATH.
- Verify that the base directory exists and has the correct permissions.
- Check the script's output for error messages or consult your system logs.

FAQ
---

**Q: Why not just use rsync?**

A: While rsync can create hard links on its own, this feature is not ideal for backups, as hard links only work within a single filesystem. Trinkup creates hard links from the latest backup directory, rather than the source, avoiding the need to cross filesystem boundaries. This makes Trinkup a more efficient solution for incremental backups.

License
-------

Trinkup is licensed under the "Do What The F*** You Want With It" (DWTFYWWI) license. See the LICENSE file for more information.

Contribution
------------

If you want to contribute to the project or report issues, please open an issue or submit a pull request on the project's GitHub page.

Contact Information
-------------------

For questions or support related to the script, use Github Issues.
