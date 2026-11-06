Kernel
======

NMI_WATCHDOG
------------
Activate kernel NMI watchdog timer:

* 0 – disabled/save power
* 1 – enabled

A value of 1 is relevant for kernel debugging and the watchdog daemon.

::

    NMI_WATCHDOG=0

Default when unconfigured: 0

.. seealso::

    * Settings: :doc:`/settings/introduction`
