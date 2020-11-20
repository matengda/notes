# options

       -a, --all
              Also list empty devices and RAM disk devices.
    
       -b, --bytes
              Print the SIZE column in bytes rather than in a human-readable format.
    
       -D, --discard
              Print information about the discarding capabilities (TRIM, UNMAP) for each device.
    
       -z, --zoned
              Print the zone model for each device.
    
       -d, --nodeps
              Do not print holder devices or slaves.  For example, lsblk --nodeps /dev/sda prints  information  about
              the sda device only.
    
       -e, --exclude list
              Exclude the devices specified by the comma-separated list of major device numbers.  Note that RAM disks
              (major=1) are excluded by default if --all is no specified.  The filter is  applied  to  the  top-level
              devices only.
    
       -f, --fs
              Output info about filesystems.  This option is equivalent to -o NAME,FSTYPE,LABEL,UUID,MOUNTPOINT.  The
              authoritative information about filesystems and raids is provided by the blkid(8) command.
              显示设备的 uuid
    
       -h, --help
              Display help text and exit.
    
       -I, --include list
              Include devices specified by the comma-separated list of major device numbers.  The filter  is  applied
              to the top-level devices only.
    
       -i, --ascii
              Use ASCII characters for tree formatting.
    
       -J, --json
              Use JSON output format.
    
       -l, --list
              Produce output in the form of a list.
    
       -m, --perms
              Output    info    about   device   owner,   group   and   mode.    This   option   is   equivalent   to
              -o NAME,SIZE,OWNER,GROUP,MODE.
    
       -n, --noheadings
              Do not print a header line.
    
       -o, --output list
              Specify which output columns to print.  Use --help to get a list of all supported columns.
    
              The default list of columns may be extended if list is specified in the format  +list  (e.g.  lsblk  -o
              +UUID).
    
       -O, --output-all
              Output all available columns.
    
       -P, --pairs
              Produce  output  in  the  form of key="value" pairs.  All potentially unsafe characters are hex-escaped
              (\x<code>).
    
       -p, --paths
              Print full device paths.
    
       -r, --raw
              Produce output in raw format.  All potentially unsafe characters  are  hex-escaped  (\x<code>)  in  the
              NAME, KNAME, LABEL, PARTLABEL and MOUNTPOINT columns.
    
       -S, --scsi
              Output info about SCSI devices only.  All partitions, slaves and holder devices are ignored.
    
       -s, --inverse
              Print dependencies in inverse order. If the --list output is requested then the lines are still ordered
              by dependencies.
    
       -t, --topology
              Output info about block-device topology.  This option is  equivalent  to  -o NAME,ALIGNMENT,MIN-IO,OPT-
              IO,PHY-SEC,LOG-SEC,ROTA,SCHED,RQ-SIZE,RA,WSAME.
    
       -V, --version
              Display version information and exit.
    
       -x, --sort column
              Sort  output  lines  by column. This option enables --list output format by default.  It is possible to
              use the option --tree to force tree-like output and than the tree branches are sorted by the column.





# SYNOPSIS

lsblk [options] [device...]



# NAME

lsblk - list block devices