tlp-stat
--------
.. topic:: Purpose

    Show TLP's configuration, system information, kernel power saving
    settings and diagnostic data.

.. rubric:: Status report with configuration and all active settings:

::

    sudo tlp-stat

.. rubric:: Show battery information:

::

    sudo tlp-stat -b
    sudo tlp-stat --battery

.. rubric:: Show configuration:

::

    tlp-stat -c
    tlp-stat --config

.. rubric:: Show configuration differing from defaults:

*Version 1.4 and higher*

::

    tlp-stat --cdiff

.. rubric:: Show disk device information:

::

    tlp-stat -d
    tlp-stat --disk

.. rubric:: Show PCIe device information:

::

    tlp-stat -e
    tlp-stat --pcie

.. rubric:: Show graphics card information:

::

        tlp-stat -g
        tlp-stat --graphics

.. rubric:: Show processor information:

::

    tlp-stat -p
    tlp-stat --processor

.. rubric:: Show radio device states:

::

    tlp-stat -r
    tlp-stat --rfkill

.. rubric:: Show system information:

::

    tlp-stat -s
    tlp-stat --system

.. rubric:: Show temperatures and fan speed:

::

    tlp-stat -t
    tlp-stat --temp

.. rubric:: Show USB device information:

::

    tlp-stat -u
    tlp-stat --usb

.. rubric:: Show more information:

::

    tlp-stat -v
    tlp-stat --verbose

.. rubric:: Show warnings:

::

    tlp-stat -w
    tlp-stat --warn

Please refer to :doc:`/faq/warnings` for details.


.. rubric:: Debugging aids

Monitor power supply udev events: ::

    tlp-stat -P
    tlp-stat --pev

Show power supply diagnostic: ::

    tlp-stat --psup

Show trace output: ::

    tlp-stat -T
    tlp-stat --trace
