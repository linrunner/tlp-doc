.. _set-drive-bay:

Drive Bay
=========
This feature enables to power off removeable optical drives contained in:

* UltraBay (ThinkPads)
* MediaBay
* Other sorts of bays

BAY_POWEROFF_ON_PRF/BAL
-----------------------
Action when activating a profile:

* 1 – power off the optical drive
* 0 – optical drive remains on

*Version 1.11 and newer* ::

    BAY_POWEROFF_ON_PRF=0
    BAY_POWEROFF_ON_BAL=0

*Version 1.10 and older* ::

    BAY_POWEROFF_ON_AC=0
    BAY_POWEROFF_ON_BAT=0

Default when unconfigured: 0

.. note::

    * Re-power the drive by releasing and reinserting the eject lever; on newer
      models push the media eject button
    * Devices other than optical drives – in particular hard disk drives – are
      not affected by this setting


BAY_DEVICE
----------
Define the optical drive device in the bay. ::

    BAY_DEVICE=sr0

Default when unconfigured: sr0

.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/disks`
