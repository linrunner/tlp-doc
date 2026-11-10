Disk Drives
===========
Does SATA ALPM cause disk corruption with Btrfs?
------------------------------------------------
No. SATA ALPM has been safe since kernel 4.15.

`Statement of BTRFS developer David Sterba
<https://www.spinics.net/lists/linux-btrfs/msg101833.html>`_:
*"It's a hardware problem leading to
filesystem corruption. It's affecting all filesystem[s] but the detection
capabilities differ, so it stuck with btrfs.
[...] the fixes to ATA subsystem have been merged to 4.15."*


Why is my HDD parking the read-write heads so frequently (clicking noises)?
---------------------------------------------------------------------------
The APM settings for the *balanced* and *power-saver* profiles ::

    DISK_APM_LEVEL_ON_BAL=128
    DISK_APM_LEVEL_ON_SAV=128

shouldn't cause any problems. Unfortunately drive vendors interpret the
APM levels quite spaciously to save power.

Solution: try values > 128. See :ref:`set-disks-apm`.

How can I prevent my HDD from starting every N minutes and immediately stopping again?
--------------------------------------------------------------------------------------
Disks react autonomously and individually to the APM level depending on the brand or model.
This reaction can even include stopping the spindle motor.

Solution: try higher APM values or disable APM with 255.

Why does my HDD not spin down?
-----------------------------------------
Stopping the system disk for extended periods of time is unlikely to work,
because applications and system daemons wake up the disk frequently. There is
no real solution for this other than changing many applications and daemons.

How can I stop my 2nd HDD?
--------------------------
To spin down after 1 minute (= 12 · 5 seconds) idle time use:

*Version 1.11 and newer* ::

    DISK_SPINDOWN_TIMEOUT_ON_PRF="0 12"
    DISK_SPINDOWN_TIMEOUT_ON_BAL="0 12"
    DISK_SPINDOWN_TIMEOUT_ON_SAV="0 12"

*Version 1.10 and older* ::

    DISK_SPINDOWN_TIMEOUT_ON_AC="0 12"
    DISK_SPINDOWN_TIMEOUT_ON_BAT="0 12"
    DISK_SPINDOWN_TIMEOUT_ON_SAV="0 12"

See :ref:`set-disks-spindown`.

.. _faq-disks-spinup:

Why does my HDD always spin up when switching from AC to battery power (and vice versa)?
----------------------------------------------------------------------------------------
TLP applies configured APM levels and spin down timeouts upon every change of
power source and upon suspend/resume. Writing the settings inevitably spins up
the disk. To prevent this behaviour either disable the settings completely by
commenting all of them (with a leading `#`) or use the special value `keep`
for the specific disk.

See :ref:`set-disks-apm`.

Why is my Crucial M4 SSD so slow on battery?
--------------------------------------------
The Crucial M4 reduces performance according to the APM level. You may disable
APM with:

*Version 1.11 and newer* ::

    DISK_APM_LEVEL_ON_PRF=""
    DISK_APM_LEVEL_ON_BAL=""
    DISK_APM_LEVEL_ON_SAV=""

*Version 1.10 and older* ::

    DISK_APM_LEVEL_ON_AC=""
    DISK_APM_LEVEL_ON_BAT=""
    DISK_APM_LEVEL_ON_SAV=""

.. _faq-odd-poweroff:

Drive Bay: why is the optical drive not powered off on battery?
---------------------------------------------------------------
Possible causes are:

.. rubric:: ThinkPad without UltraBay

SL/Edge models for instance.

.. rubric:: Incompatible Laptop

At the time of writing this functionality is tested with ThinkPads only.
