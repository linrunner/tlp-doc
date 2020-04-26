File System
===========

DISK_IDLE_SECS_ON_AC/BAT
------------------------
::

    DISK_IDLE_SECS_ON_AC=0
    DISK_IDLE_SECS_ON_BAT=2

Seconds laptop mode waits after the disk goes idle before syncing dirty cache
blocks from RAM to disk again. Values > 0 activate kernel laptop mode. Do not
change this setting.

Default when unconfigured: 0 (AC), 2 (battery)


MAX_LOST_WORK_SECS_ON_AC/BAT
----------------------------
::

    MAX_LOST_WORK_SECS_ON_AC=15
    MAX_LOST_WORK_SECS_ON_BAT=60

Timeout (in seconds) for writing unsaved data in file system buffers to disk.

Default when unconfigured: 15 (AC and battery)
