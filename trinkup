#!/bin/bash
#
# trinkup - TRivial INcremental bacKUP script
#
# Licensed under DWTFYWWI license, but recommended to be used before drinkup.
#

set -e

if ! which rsync >/dev/null; then
   echo "rsync not found!" >&2
   exit 1
fi

USAGE="
usage:

    $0 <rsync_source> <backups_directory> <number_of_backups> [rsync_args]

    Keep calm and drink vodka!
"

SRC="$1"
BACKUPS="$2"
ROTATE="$3"

if [ -z "$SRC" ] || [ -z "$BACKUPS" ] || ! [ -n "$ROTATE" -a "$ROTATE" -gt 1 ] ; then
    echo -e "$USAGE" >&2
    exit 1
fi

shift 3

if ! [ -d "$BACKUPS" ] ; then
    echo "'$BACKUPS' is not a directory!" >&2
    exit 1
fi

WORKDIR="$(echo "$BACKUPS/`basename \"$SRC\"`.`date +%s`" | sed 's,/\+,/,g')"

echo "Using '$WORKDIR' as backup destination."

if [ -d `ls -rtd \"$BACKUPS/$(basename "$SRC")\"\.[0-9]* 2>/dev/null | head -n 1` ] ; then
    OLDEST="`ls -rtd \"$BACKUPS/$(basename "$SRC")\"\.[0-9]* | sed 's,/\+,/,g' | awk NR==$ROTATE`"
    NEWEST="`ls -rtd \"$BACKUPS/$(basename "$SRC")\"\.[0-9]* | sed 's,/\+,/,g' | head -n 1`"
fi

if [ -d "$OLDEST" ] ; then
    echo "Reusing $OLDEST to reduce amount of disk operations."
    mv "$OLDEST" "$WORKDIR"
fi

if [ -d "$NEWEST" ] ; then
    echo "Syncing with $NEWEST..."
    cp -laf "$NEWEST/." "$WORKDIR"
fi

echo "Running rsync..."
rsync -a --delete $* "$SRC" "$WORKDIR"

echo "Done."