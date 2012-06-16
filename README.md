hsync
=====

rsync-like utility to copy HDFS files between Hadoop clusters.

## Motivation

Transferring files between Hadoop clusters today is best supported using
[distCp](http://hadoop.apache.org/common/docs/r1.0.0/distcp.html).
The goal of hsync is to provide a file copying utility which closely mirrors
the capabilities of rsync, and provide features that are not supported in DistCp
today such as a daemon capability, exclude/include patterns and
dry runs to name a few.

## Features

The hsync features include:

* Leveraging MapReduce to copy files between Hadoop clusters in parallel
* Copying owners, groups and permissions
* Exclude and exclude-from options similar to GNU tar
* Daemon mode for continuous operation


## License

Apache License, Version 2.0.

## Installation


## Options Summary

Options which require an update to distCp marked by `*`. distCp options not supported by rsync marked
with `~`.

<pre><code>
-v, --verbose               * increase verbosity
-c, --checksum              skip based on checksum, not mod-time & size
-p, --perms                 preserve permissions
-E, --executability         preserve executability
    --chmod=CHMOD           * affect file and/or directory permissions
-o, --owner                 preserve owner (super-user only)
-g, --group                 preserve group
-a, --archive               * archive mode; equals -rtgo
-r, --recursive             * recurse into directories
-u, --update                * skip files that are newer on the receiver
-t, --times                 * preserve modification times
-n, --dry-run               * perform a trial run with no changes made
-I, --ignore-times          * don't skip files that match size and time
    --size-only             * skip files that match in size
--ignore-existing           * skip updating files that exist on receiver
--remove-source-files       * sender removes synchronized files (non-dir)
--delete                    delete extraneous files from dest dirs
--delete-before             receiver deletes before transfer (default)
--delete-after              * receiver deletes after transfer, not before
--force                     * force deletion of dirs even if not empty
--max-delete=NUM            * don't delete more than NUM files
--max-size=SIZE             don't transfer any file larger than SIZE
--min-size=SIZE             * don't transfer any file smaller than SIZE
-T, --temp-dir=DIR          create temporary files in directory DIR
-f, --filter=RULE           * add a file-filtering RULE
--exclude=PATTERN           * exclude files matching PATTERN
--exclude-from=FIL          * read exclude patterns from FILE
--include=PATTERN           * don't exclude files matching PATTERN
--include-from=FIL          * read include patterns from FILE
--files-from=FILE           read list of source-file names from FILE
--preserve-replication      ~ preserve replication count
--preserve-block-size       ~ preserve replication count
--ignore-failures           ~ continue the copy even if a map task fails
--log-dir                   ~ HDFS directory containing file copy logs
--max-parallel-copies       ~ the number of map tasks used for the copy
--max-num-files             ~ the maximum number of files copied in one map job
(-h) --help                 show this help (see below for -h comment)
</code></pre>


Hsync can also be run as a daemon, in which case the following options are accepted:

<pre><code>
--daemon            run as a daemon
--config            specify
--log-file          specify the location of the log files
-v, --verbose       increase verbosity
</code></pre>

## Usage Examples

### Recursively synchronize a directory preserving permissions, owner and group

The `-a` option is euqal to using the `rtgo` options.

<pre><code>shell$ hsync -a hdfs://A:8020//src/path/ hdfs://B:8020//dest/path/
</code></pre>

### Run hsync as a daemon with the options specified in `hsync.conf`

<pre><code>shell$ hsync --daemon --config=hsync.conf
</code></pre>
