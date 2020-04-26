.. _set-drive-bay:

Drive Bay
=========
This feature enables to power off removeable optical drives contained in:

* UltraBay (ThinkPads)
* MediaBay
* Other sorts of bays

BAY_POWEROFF_ON_AC/BAT
----------------------
::

    BAY_POWEROFF_ON_AC=0
    BAY_POWEROFF_ON_BAT=0

* 1 – power off the optical drive
* 0 – optical drive remains on

Default when unconfigured: 0

BAY_DEVICE
----------
::

    BAY_DEVICE=sr0

Device for the optical drive.

Default when unconfigured: sr0

.. note::

    * Re-power the drive by releasing and reinserting the eject lever; on newer
      models push the media eject button
    * Devices other than optical drives – in particular hard disk drives – are
      not affected by this setting
