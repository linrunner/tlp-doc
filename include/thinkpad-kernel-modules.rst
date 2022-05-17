.. important::

    As of version 5.17, the Linux kernel in combination with TLP 1.5 offers
    full battery care support (i.e. charge thresholds and recalibration) for
    ThinkPads from model year 2011 onwards. Therefore no external kernel modules
    are required with kernel 5.17 or newer and you do not need to proceed any
    further here.

    Linux kernel 4.19 through 5.16 provides only charge threshold functionality
    but no recalibration. If this is sufficient for you, stop reading here.

    However, if you need the recalibration feature or your model and/or kernel
    is older, read on.

    You may find out your current kernel version with the command
    :command:`uname -a` or when TLP is already installed with
    :command:`tlp-stat -s`.
