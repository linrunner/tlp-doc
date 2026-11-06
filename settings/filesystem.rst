File System
===========

DISK_IDLE_SECS_ON_AC/BAT
------------------------
Seconds laptop mode waits after the disk goes idle before syncing dirty cache
blocks from RAM to disk again. Values > 0 activate kernel laptop mode. Do not
change this setting.

*Version 1.11 and newer* ::

    DISK_IDLE_SECS_ON_PRF=0
    DISK_IDLE_SECS_ON_BAL=2

*Version 1.10 and older* ::

    DISK_IDLE_SECS_ON_AC=0
    DISK_IDLE_SECS_ON_BAT=2

Default when unconfigured: 0 (PRF/AC), 2 (BAL/BAT)

.. note::

    The `laptop_mode` feature has been disabled in kernel 7.0 and will be removed
    in a *future TLP version*. The corresponding kernel message
    *"tlp: vm.laptop_mode is deprecated. Ignoring setting."* can be safely ignored.


MAX_LOST_WORK_SECS_ON_AC/BAT
----------------------------
Timeout (in seconds) for writing unsaved data in file system buffers to disk.

*Version 1.11 and newer* ::

    MAX_LOST_WORK_SECS_ON_PRF=15
    MAX_LOST_WORK_SECS_ON_BAL=15
    MAX_LOST_WORK_SECS_ON_SAV=15

Default when unconfigured: 15

*Version 1.10 and older* ::

    MAX_LOST_WORK_SECS_ON_AC=15
    MAX_LOST_WORK_SECS_ON_BAT=60

Default when unconfigured: 15 (AC), 60 (BAT)


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * Settings: :doc:`/settings/disks`
    * FAQ: :doc:`/faq/disks`
