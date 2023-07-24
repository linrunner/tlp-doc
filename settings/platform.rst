Platform
========

.. _set-platform-profile:

PLATFORM_PROFILE_ON_AC/BAT
--------------------------
*Version 1.4 and newer*

::

    PLATFORM_PROFILE_ON_AC=performance
    PLATFORM_PROFILE_ON_BAT=low-power

Select the platform profile to control system operating characteristics around
power/performance levels, thermal and fan speed. Possible values in order of
increasing power saving are:

* performance
* balanced
* low-power

.. note::

    Check the output of :command:`tlp-stat -p` to determine availability on your
    hardware and additional profiles such as `balanced-performance`, `quiet`, `cool`.

.. seealso::

    * `Platform Profile Selection <https://www.kernel.org/doc/html/latest/userspace-api/sysfs-platform_profile.html>`_
      – kernel API documentation
    * `Linux 5.12 Should See ACPI Platform Profile Support To Alter System Thermal/Power Levels <https://www.phoronix.com/scan.php?page=news_item&px=Linux-ACPI-Platform-Profile>`_
      - news article
