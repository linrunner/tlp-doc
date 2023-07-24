tlp-stat
--------
.. topic:: Purpose

    View TLP's configuration, system information, kernel power saving settings
    and battery data.

.. rubric:: Invocation without options shows all information categories

::

    sudo tlp-stat


.. rubric:: View battery data

::

    sudo tlp-stat -b
    sudo tlp-stat --battery

Add `-v` to see battery voltages (if available).


.. rubric:: View active configuration

::

    tlp-stat -c
    tlp-stat --config


.. rubric:: View the difference between defaults and user configuration

*Version 1.4 and newer*

::

    tlp-stat --cdiff


.. rubric:: View disk device information

::

    sudo tlp-stat -d
    sudo tlp-stat --disk


.. rubric:: View PCIe device information

::

    sudo tlp-stat -e
    sudo tlp-stat --pcie

*Version 1.4 and newer*: add `-v` to see device runtime status.


.. rubric:: View graphics card information

::

    sudo tlp-stat -g
    sudo tlp-stat --graphics


.. rubric:: View processor information

::

    sudo tlp-stat -p
    sudo tlp-stat --processor

* *Version 1.4 and newer*: for clarity the standard output shows only `cpu0`,
  add  `-v` to see all.
* *Version 1.6 and newer*: `-v` will also show `acpi_cppc` performance and
  frequency attributes.

.. rubric:: View radio device states

::

    tlp-stat -r
    tlp-stat --rfkill


.. rubric:: View system information

::

    tlp-stat -s
    tlp-stat --system


.. rubric:: View temperatures and fan speed

::

    tlp-stat -t
    tlp-stat --temp


.. rubric:: View USB device information

::

    tlp-stat -u
    tlp-stat --usb

*Version 1.4 and newer*: add `-v` to see device runtime status.


.. rubric:: Show more information in battery, PCIe, processor and USB categories

::

    tlp-stat -v
    tlp-stat --verbose


.. rubric:: Diagnostics and debugging

Monitor power supply udev events: ::

    sudo tlp-stat -P
    sudo tlp-stat --pev

View power supply diagnostics: ::

    tlp-stat --psup

View trace output: ::

    sudo tlp-stat -T
    sudo tlp-stat --trace

Check if udev rules for power source changes and connecting USB devices are active:

*Version 1.4 and newer*

::

    tlp-stat --udev

View warnings about SATA disks: ::

    tlp-stat -w
    tlp-stat --warn

Please refer to :doc:`/faq/warnings` for details.
